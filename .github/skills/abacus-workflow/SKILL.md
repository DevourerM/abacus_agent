---
name: abacus-workflow
description: "Use when generating or checking ABACUS INPUT, KPT, STRU, SCF/NSCF/relax/band/DOS workflows, pseudopotential and numerical-orbital paths, MPI/OpenMP settings, LSF scripts, job status, or ABACUS error logs."
---

# ABACUS 科研工作流

## 最小检查原则

- 只有在版本、参数兼容性或报错诊断需要时，才检查 ABACUS 版本；优先读取已有日志，不要默认执行 `abacus --version` 或加载 module。
- 只有在生成/提交/诊断任务需要时，才检查 `INPUT`、`STRU` 和资源路径；不要重复检查用户已经明确提供且未变化的路径。
- `INPUT-scf`、`INPUT-nscf` 是阶段模板，运行前需复制为严格文件名 `INPUT`。
- `basis_type=lcao` 时必须有数值轨道；`basis_type=pw` 通常不需要轨道目录。
- 检查坐标单位、晶格常数单位（Bohr）、能量截断单位（Ry）、k 点坐标和权重。

## 阶段规则

- SCF：从原子电荷开始可用 `init_chg atomic`；记录收敛阈值和最终 `OUT.<suffix>`。
- NSCF：必须有可用的 SCF 电荷/波函数，常用 `init_chg file`；不得覆盖尚未备份的 SCF 结果。
- Band：使用 `KPT` 的 `Line`/`Line_Cartesian`，确保 `calculation=nscf` 及输出能带参数匹配。
- 能带输出优先按实际目录探测：常见文件包括 `OUT.<suffix>/band.txt`、`BANDS_*.dat`、`eig*.txt`；不能写死单一文件名，也不能因缺少 `BANDS_1.dat` 就判定 NSCF 失败。
- 能带条数由 `band.txt` 的列数或 `INPUT` 的 `nbands` 决定。绘图脚本应读取实际列数，不得人为截断；若参考图更多能带，应提高 SCF 和 NSCF 的 `nbands` 后重新计算。
- LCAO HSE：根据当前版本使用 `exx_fock_alpha`、`exx_erfc_alpha`、`exx_erfc_omega` 等已注册参数，不能套用旧版本名称。

## 并行与 LSF

总核数满足：`LSF 核数 = MPI ranks * OMP_NUM_THREADS`。保守默认是 `OMP_NUM_THREADS=1`。
提交前才检查可执行文件、环境脚本、`INPUT`、`STRU` 和队列资源。提交、取消作业必须先让用户确认具体命令。环境加载输出不是 ABACUS 错误。

## 诊断顺序

1. `bjobs`/`bjobs -l` 判断 PEND、RUN、DONE、EXIT。
2. 查看 `abacus.<jobid>.out`、`.err`、`.log` 和 `OUT.<suffix>/running_*.log`。
3. 区分参数拼写错误、资源路径错误、输入结构错误、MPI/环境错误和 SCF 不收敛；先用本地已有日志，不重复执行环境初始化。
4. 用当前可执行文件运行 `--check-input`（若该版本支持），再做最小计算或正式提交。

## 后处理脚本

用户未提供绘图脚本时，按实际输出自动生成 Python 脚本：探测 `band.txt`、`BANDS_*.dat` 和 `eig*.txt`，从 `running_nscf.log`/输出日志读取费米能级，按实际能带列数绘制，并从 KPT line-mode 自动生成高对称点刻度。脚本必须报告输入文件、能带数、k 点数、费米能级和输出图路径；缺少某个候选文件时只尝试下一个候选，不直接报错。

## 输出要求

每次报告包含：变更文件、关键参数和单位、执行的验证命令及结果、尚未验证的物理风险。不要把“检查通过”描述成“科学结果正确”。
