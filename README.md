# ABACUS Research Agent

这是一个面向科研计算的 Copilot/Codex 自定义 agent 包，用于 ABACUS 输入文件生成、检查、集群作业配置和错误诊断。

## 目录

- `.github/agents/abacus-research.agent.md`：可在 VS Code agent picker 中选择的主 agent。
- `.github/skills/abacus-workflow/SKILL.md`：ABACUS 工作流规则和检查顺序。
- `.github/skills/abacus-workflow/templates/`：带中文注释和 `【改我】` 标记的 INPUT、KPT、STRU、LSF 模板。
- `.github/skills/abacus-workflow/references/official-resources.md`：官方文档、源码和版本证据策略。
- `REPORT.md`：本次制作报告。

## 使用

将该目录作为一个 VS Code 工作区打开，在 Agent picker 中选择 `ABACUS Research Agent`。可以直接提供 CIF、POSCAR、XYZ、STRU、INPUT、KPT、赝势/轨道目录或报错日志，并说明计算目标。

推荐请求包含：体系结构、计算类型、泛函、PW/LCAO、k 点、精度目标、可用节点/队列和是否允许生成提交命令。Agent 会先检查资料和版本，再生成文件；提交或取消作业必须由用户明确授权。

## 重要约定

- ABACUS 运行入口默认读取严格命名为 `INPUT` 的文件；`INPUT-scf` 等只是阶段模板。
- `STRU` 中写赝势和轨道文件名，`INPUT` 中的目录必须能与这些文件名拼接成功。
- 单位必须显式核对：晶格常数通常为 Bohr，`ecutwfc` 为 Ry，k 点坐标依 KPT 模式确定。
- 计算收敛、输入通过检查和科研结果可信是三个不同结论。
