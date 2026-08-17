# RUN019 栅下负固定界面电荷低量级响应

- 日期：2026-08-18
- 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`
- 材料身份：`BetaGa2O3`
- 阶段：正向 `VDS=1 V` 转移拟合；未运行 BV 或 SEB
- 母版：RUN018，唯一数值变化为 `Qf=-1e12 → -1e11 cm^-2`

## 冻结定义

```text
interface qf=-1.0e11 x.min=4.00 x.max=7.00 y.min=0.00 y.max=0.15
```

界面窗口仍只覆盖栅金属投影内的 Al2O3/BetaGa2O3 界面。Gate work function 保持 `5.78 eV`；结构、网格、材料、掺杂、陷阱、Dit、迁移率和求解器均未改变。

## 正式路径与状态

- 远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN019_GATE_INTERFACE_QF_NEG1E11/`
- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN019_GATE_INTERFACE_QF_NEG1E11/YUE2026_RUN019_GATE_INTERFACE_QF_NEG1E11.in`
- 最新 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN019_GATE_INTERFACE_QF_NEG1E11/YUE2026_RUN019_GATE_INTERFACE_QF_NEG1E11_vd1_vg12.str`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN019_GATE_INTERFACE_QF_NEG1E11/YUE2026_RUN019_GATE_INTERFACE_QF_NEG1E11.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN019_GATE_INTERFACE_QF_NEG1E11/YUE2026_RUN019_GATE_INTERFACE_QF_NEG1E11.typescript`
- tmux：`yue_run019_qfneg1e11_20260818`，已结束
- 进程：DeckBuild 与 `atlas.exe2` 均已退出
- 正式点数：121；最后实际偏压 `VGS=12.0 V`
- ATLAS 正常结束；计算段 `Error(s)=0`、`Warning(s)=0`
- 最大绝对 KCL：`7.728e-17 A`

## 对比结果

| 指标 | RUN012，无 Qf | RUN019，Qf=-1e11 cm^-2 | 变化 |
|---|---:|---:|---:|
| 1 μA 交点 | 1.906984 V | 1.956600 V | +0.049616 V |
| 2 μA 交点 | 1.953404 V | 2.025246 V | +0.071843 V |
| 20 μA 阈值 | 3.064425 V | 4.180843 V | +1.116418 V |
| ID@3.1 V | 20.0813 μA | 18.0901 μA | -9.92% |
| ID@9 V | 23.1194 μA | 22.5574 μA | -2.43% |
| ID@12 V | 23.4709 μA | 23.0916 μA | -1.62% |
| SS，1e-12..1e-8 A | 73.2966 mV/dec | 73.3114 mV/dec | +0.02% |
| SS，1e-10..1e-6 A | 81.4829 mV/dec | 82.0906 mV/dec | +0.75% |

RUN019 在 1 pA 以上无非单调，且没有 RUN018 的平台。12 V 电流远高于 20 μA，下降幅度也远低于 15%，因此是可用低量级响应。

## 判定与下一步边界

`Qf=-1e11 cm^-2` 能在保持高栅压电流和 SS 的同时，把 20 μA 阈值正移约 1.116 V；但 1 μA、2 μA 交点只移动约 0.05–0.07 V，说明它不是全电流范围的刚性平移。下一步必须继续监控曲线形状，不能按 1.116 V 的比例直接跳到约 `-5e11 cm^-2`。

下一运行只允许一个受控中间 Qf 单点；仍以完整 12 V、`ID@12 V≥20 μA`、SS 变化约不超过 10%、无平台/非单调/收敛退化为硬门槛。网页端只审核下一步量级与停止判据，不代写 IN。
