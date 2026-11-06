# OpenArmX Sim

双臂仿真资产搭建、脚本控制策略与 LeRobot 导出 Demo。

## v0.2.0：补齐 LeRobot 导出

旧 v0.1.0 只有原始 HDF5、三相机 MP4 与英语事件时间线，未提供 LeRobot 导出入口。本次补齐独立 CPU 导出器，以及采集结束后自动导出的可选入口。

[下载完整源码 v0.2.0](openarmx-sim-demo-source-v0.2.0.zip)。沿用 v0.1.0 的冻结源码 ZIP 交付方式：**先解压，再运行包内脚本**；源码没有散放在 GitHub 仓库根目录。

```bash
unzip openarmx-sim-demo-source-v0.2.0.zip
cd openarmx-sim-demo
```

解压后查看 `docs/LEROBOT.md`，在独立 Python 3.10 CPU 环境安装 `requirements-export.txt`；不要装进 Isaac Sim 环境。

```bash
EXPORT_PY=/path/on/data-volume/export-env/bin/python
RAW=/path/on/data-volume/demo-A-001
OUT=/path/on/data-volume/lerobot/demo-A-001

# 转换已经成功结束、完整封盘的 CTX01 三相机原生录制。
"$EXPORT_PY" -B scripts/export_lerobot.py \
  --source "$RAW" --output "$OUT" --repo-id example/openarmx-ctx01-demo
```

也可用包内 `scripts/demo.py --lerobot-output ... --lerobot-python ... --repo-id ...`，在采集子进程成功退出后独立导出；具体资产、cache、输出和环境参数见完整指南。

- 使用官方 `lerobot==0.3.3`，输出标准 **LeRobot Dataset v2.1**；不是 v3，也不是私有修改版运行时。
- 三相机原生 30 Hz / 640×480 MP4 同字节复制，不暂存 PNG、不二次编码。
- 按关节名映射 16 维 state/action，保留 rad/m 单位、原始监督 mask 与完整尾段。
- 每帧 `task` 包含公共协议及截至该帧已经到达的完整英语历史，原始逐帧标注另存；不提前泄露未来指令。
- 导出后使用官方 CPU reader 实际回读，只有通过才写 `FORMAT_READY.json`；格式通过不等于正式训练准入，不自动上传或训练。

## 资产与控制示例

源码包含原生 USD 资产准备与指纹检查、场景组合、关节链 FK/IK、有界运动规划、PD 驱动以及接触/释放检查。本示例只开放 **CTX01 A/B 操作历史选择任务**，控制器是脚本专家，不是训练好的 VLA/RL 模型。

```bash
# 只查看任务计划，不启动仿真。
python -B scripts/demo.py --plan-only --variant A
python -B scripts/demo.py --plan-only --variant B
```

完整物理运行需要兼容的 Isaac Sim 4.5 / Python 3.10 环境及合法持有的原生资产，见 ZIP 内 `README.md` 和 `docs/ASSETS.md`。仿真采集与导出分别使用独立环境。此更新未修改冻结物理采集代码或验收阈值；不宣称重新运行过 GPU/PhysX。

[历史源码 v0.1.0](openarmx-sim-demo-source-v0.1.0.zip) 保留不变，`SOURCE_MANIFEST.json` 对应 v0.2.0 ZIP 内的源码清单与 SHA256。没有上传真实采集数据、数据派生校验明细、录像、USD/mesh/纹理、模型权重、密钥或真实机器人项目。

本初稿未新增统一开源许可证。代码、第三方资产、数据和模型许可分开处理，源码发布不授予第三方资产再分发或商用授权。
