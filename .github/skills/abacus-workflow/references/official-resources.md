# ABACUS 权威资料与版本策略

## 官方资料

- 官方文档首页：https://abacus.deepmodeling.com/en/latest/
- INPUT 参数全集：https://abacus.deepmodeling.com/en/latest/advanced/input_files/input-main.html
- STRU 格式：https://abacus.deepmodeling.com/en/latest/advanced/input_files/stru.html
- KPT 格式：https://abacus.deepmodeling.com/en/latest/advanced/input_files/kpt.html
- 官方源码仓库：https://github.com/deepmodeling/abacus-develop
- 赝势与数值轨道说明：https://abacus.deepmodeling.com/en/latest/advanced/input_files/stru.html
- ABACUS 官网：http://abacus.ustc.edu.cn/

## 证据优先级

1. 用户当前运行的可执行文件版本和 `--check-input` 结果。
2. 与该版本对应的本地源码和本地文档。
3. 官方在线文档，并记录访问日期和 URL。
4. 官方示例；示例可能滞后，不能覆盖本地版本实际行为。

例如，当前 v3.11 系列源码注册 HSE 参数为 `exx_erfc_alpha` 和 `exx_erfc_omega`，不能因旧示例出现 `exx_hybrid_alpha` 就继续使用旧名。

## 结果可信度记录

生成报告时记录：ABACUS 版本、git commit（若可得）、编译器/MPI、输入文件哈希或版本、赝势/轨道名称、ecutwfc、k 点、smearing、SCF 阈值、MPI/线程数和收敛状态。对 ecutwfc、k 点、nbands 和 SCF 阈值做收敛测试后，才讨论物理结论。
