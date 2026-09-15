---
name: abacus-workflow
description: "Use when generating or checking ABACUS INPUT, KPT, STRU, SCF/NSCF/relax/band/DOS workflows, pseudopotential and numerical-orbital paths, MPI/OpenMP settings, LSF scripts, job status, or ABACUS error logs."
---

# ABACUS 科研工作流

## 必须先做的检查

- 确认当前 ABACUS 版本：`abacus --version` 或运行日志中的版本。
- 确认工作目录有标准文件名 `INPUT`、`STRU`；带后缀的 `INPUT-scf`、`INPUT-nscf` 只是模板，运行前需复制为 `INPUT`。
- 从 `STRU` 读取赝势和轨道文件名，并与 `pseudo_dir`、`orbital_dir` 拼接检查。
- `basis_type=lcao` 时必须有数值轨道；`basis_type=pw` 通常不需要轨道目录。
- 检查坐标单位、晶格常数单位（Bohr）、能量截断单位（Ry）、k 点坐标和权重。

## 阶段规则

- SCF：从原子电荷开始可用 `init_chg atomic`；记录收敛阈值和最终 `OUT.<suffix>`。
- NSCF：必须有可用的 SCF 电荷/波函数，常用 `init_chg file`；不得覆盖尚未备份的 SCF 结果。
- Band：使用 `KPT` 的 `Line`/`Line_Cartesian`，并确保 `calculation=nscf` 及输出能带参数匹配。
- LCAO HSE：根据当前版本使用 `exx_fock_alpha`、`exx_erfc_alpha`、`exx_erfc_omega` 等已注册参数，不能套用旧版本名称。

## 并行与 LSF

总核数满足：`LSF 核数 = MPI ranks * OMP_NUM_THREADS`。保守默认是 `OMP_NUM_THREADS=1`。
提交前检查可执行文件、环境脚本、`INPUT`、`STRU` 和队列资源。提交、取消作业必须先让用户确认具体命令。

## 诊断顺序

1. `bjobs`/`bjobs -l` 判断 PEND、RUN、DONE、EXIT。
2. 查看 `abacus.<jobid>.out`、`.err`、`.log` 和 `OUT.<suffix>/running_*.log`。
3. 区分参数拼写错误、资源路径错误、输入结构错误、MPI/环境错误和 SCF 不收敛。
4. 用当前可执行文件运行 `--check-input`（若该版本支持），再做最小计算或正式提交。

## 输出要求

每次报告包含：变更文件、关键参数和单位、执行的验证命令及结果、尚未验证的物理风险。不要把“检查通过”描述成“科学结果正确”。
