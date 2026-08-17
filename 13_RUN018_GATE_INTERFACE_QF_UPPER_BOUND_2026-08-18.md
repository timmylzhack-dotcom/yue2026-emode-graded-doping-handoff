# RUN018 栅下负固定界面电荷上界

- 日期：2026-08-18
- 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`
- 材料身份：`BetaGa2O3`
- 阶段：正向 `VDS=1 V` 转移拟合；未运行 BV 或 SEB
- 母版：RUN012，gate work function 保持 `5.78 eV`

## 唯一变量与界面范围

```text
interface qf=-1.0e12 x.min=4.00 x.max=7.00 y.min=0.00 y.max=0.15
```

该窗口只覆盖栅金属投影内的 Al2O3/BetaGa2O3 界面，包括凹槽底面、两侧壁和栅头覆盖下的台面界面；不覆盖 SiO2 钝化、源场板或漏侧界面。结构、网格、材料卡、掺杂、陷阱、功函数、Dit、迁移率和求解器全部冻结。

## 正式路径与状态

- 远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN018_GATE_INTERFACE_QF_NEG1E12/`
- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN018_GATE_INTERFACE_QF_NEG1E12/YUE2026_RUN018_GATE_INTERFACE_QF_NEG1E12.in`
- 最新 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN018_GATE_INTERFACE_QF_NEG1E12/YUE2026_RUN018_GATE_INTERFACE_QF_NEG1E12_vd1_vg12.str`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN018_GATE_INTERFACE_QF_NEG1E12/YUE2026_RUN018_GATE_INTERFACE_QF_NEG1E12.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN018_GATE_INTERFACE_QF_NEG1E12/YUE2026_RUN018_GATE_INTERFACE_QF_NEG1E12.typescript`
- tmux：`yue_run018_qfneg1e12_20260818`，已结束
- 进程：DeckBuild 与 `atlas.exe2` 均已退出
- 正式点数：121；最后实际偏压 `VGS=12.0 V`
- ATLAS 正常结束；计算段 `Error(s)=0`、`Warning(s)=0`
- 最大绝对 KCL：约 `1.00e-16 A`

## 对比结果

| 指标 | RUN012，无 Qf | RUN018，Qf=-1e12 cm^-2 |
|---|---:|---:|
| ID@3.1 V | 20.0813 μA | 0.009334 μA |
| ID@9 V | 23.1194 μA | 0.895247 μA |
| ID@12 V | 23.4709 μA | 1.321748 μA |
| 1 μA 交点 | 1.906984 V | 9.742289 V |
| 2 μA 交点 | 1.953404 V | 未达到 |
| 20 μA 阈值 | 3.064425 V | 未达到 |
| SS，1e-12..1e-8 A | 73.2966 mV/dec | 375.3684 mV/dec |
| SS，1e-10..1e-6 A | 81.4829 mV/dec | 1923.5106 mV/dec |

RUN018 在 1 pA 以上仍单调，且数值收敛本身正常；失败来自物理响应过强和明显平台，不是网格或求解器失败。

## 停止判定

`Qf=-1e12 cm^-2` 同时触发 `ID@12 V<20 μA`、扫描内没有 20 μA 阈值交点以及 SS/平台严重形变，故锁定为过补偿上界。不能把 9.742 V 的 1 μA 交点冒充论文采用的 20 μA（1 mA/mm）阈值。

下一单点保持相同界面窗口，把 `|Qf|` 降一个数量级至 `1e11 cm^-2`，先验证高栅压电流、SS 和曲线形状是否恢复。不得继续增大负 Qf，也不得用 MAXTRAPS/ITLIMIT 掩盖物理过补偿。
