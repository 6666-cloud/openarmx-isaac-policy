# OpenArmX Sim

## 双臂仿真资产搭建与脚本控制策略 Demo

**Native scene assets and contact-driven scripted control for bimanual manipulation**

首个源码示例包已上传：[下载源码 v0.1.0](openarmx-sim-demo-source-v0.1.0.zip)。本次以冻结 ZIP 交付完整目录，下载解压后查看 `openarmx-sim-demo/README.md`；源码没有散放在仓库根目录。

## 包含什么

- **资产搭建**：原生 USD 机器人、桌面、积木与盘子的组合代码，以及本地资产准备器、几何/物理属性指纹与依赖检查。
- **控制 policy**：脚本专家状态机、USD 关节链 FK/IK、有界运动规划、关节 PD 驱动和接触/释放检查。不是训练好的 VLA/RL 模型。
- **一个 demo**：CTX01 操作历史选择。先抓起一块积木、保持并放回，再把之前没有操作的另一块放到中央盘中；提供 A/B 分支。
- **使用说明**：环境、资产、控制接口、运行命令、许可和验证边界。
- **来源清单**：每个冻结源码和适配文件的 SHA256，见压缩包及仓库中的 `SOURCE_MANIFEST.json`。

## 先看无需 GPU 的示例

```bash
unzip openarmx-sim-demo-source-v0.1.0.zip
cd openarmx-sim-demo
python -B scripts/demo.py --plan-only --variant A
python -B scripts/demo.py --plan-only --variant B
```

此命令只打印英语指令、任务计划和期望结果，不启动仿真、不生成训练数据。

完整物理运行需要兼容的 **Isaac Sim 4.5 / Python 3.10** 环境和合法持有的原生资产；步骤在解压后的 README 与 `docs/ASSETS.md`。第三方 USD/mesh/纹理不在这个源码 ZIP 中。仅克隆本仓库不能直接获得完整资产或运行物理仿真。

## 验证与范围

- 46 个源码/说明/配置文件，未压缩共 607,293 bytes。
- **36 项 CPU 回归测试通过**；本次没有重跑 GPU/PhysX 完整 episode。
- 本地原生资产准备和尺寸/SHA 校验已完成，几何、变换与物理属性指纹保持一致；这不是接触物理成功认证。
- 不包含数据集、录像、模型权重、密钥、服务器地址或真实机器人项目。
- 此版从 `context_v07_r10_ctx01_retime12_v1` 提取，仅开放 CTX01 demo，不是全任务生产系统或正式训练验收包。

源码 ZIP SHA256：

```text
fa316340c228cee845cca2600f192d0db67bdd56508d94a07396a1ddc2eed882
```

代码、资产和数据许可分开处理，本初稿未新增统一开源许可证。第三方资产遵循各自原始许可，不能因源码发布而视为获得模型/纹理再分发或商用授权。
