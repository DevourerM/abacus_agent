# 制作报告

## 目标

建立一个可被 Copilot/Codex 读取的 ABACUS 科研计算 agent，支持结构/已有文件读取、INPUT/KPT/STRU 生成、LSF 配置、作业诊断，以及官方资料核查。

## 已完成

- 创建标准自定义 agent：`.github/agents/abacus-research.agent.md`。
- 创建按需技能：`.github/skills/abacus-workflow/SKILL.md`。
- 创建带注释和 `【改我】` 标记的四类模板：INPUT、KPT、STRU、LSF。
- 加入 ABACUS 官方手册、官网、源码仓库链接和版本证据优先级。
- 规定输入文件、资源路径、单位、并行、收敛和可复现性检查。
- 规定提交/取消 LSF 作业前必须得到用户明确确认，避免科研工作流中的意外副作用。

## 使用反馈后的优化

- 将环境和版本检查改为按需执行：优先读取用户已有日志和文件，避免无必要的 module 加载和重复检查。
- 将能带输出识别改为候选文件探测，接受 `OUT.<suffix>/band.txt`、`BANDS_*.dat` 和 `eig*.txt`，不再写死 `BANDS_1.dat`。
- 明确能带条数应由实际输出列数和 `nbands` 决定；绘图脚本不得人为截断，能带不足需要提高 SCF/NSCF 的 `nbands` 并重新计算。
- 增加没有现成绘图脚本时自动生成 Python 后处理脚本的规则，要求报告输入文件、k 点数、能带数、费米能级和输出图路径。
- 将以上规则同步到当前工作区根目录的 `.github/agents/abacus-research.agent.md`。

## 已核对资料

本次参考了 ABACUS 官方文档中的 INPUT、STRU、KPT 页面，以及当前工作区 ABACUS 源码中的参数注册。特别确认当前源码的 HSE 参数使用 `exx_erfc_alpha` 和 `exx_erfc_omega`。

## 验证

- 目录结构和标准 frontmatter 已生成。
- 模板内容采用 UTF-8 中文注释，Shell 模板使用 Bash 语法结构。
- 未提交或取消任何集群作业。
- 未复制整套官网内容；使用官方 URL 索引，降低资料过期和重复维护风险。

## 未覆盖范围

- 没有假设所有集群都有相同的队列名、module 名称或 MPI 启动参数；使用时需由 agent 根据用户环境核查。
- 当前没有加入自动 CIF/POSCAR 解析程序；agent 可先读取文件并生成输入，后续如需批量处理可增加独立脚本和测试。
- 物理结果不会由 agent 自动判定为发表级；仍需用户进行收敛测试和科学审查。
