# RUN020 槽底限定负固定电荷验收

- 日期：2026-08-18
- 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`
- 材料身份：`BetaGa2O3`
- 阶段：正向 `VDS=1 V` 转移拟合；未运行 BV 或 SEB
- 母版：RUN012

## 对 RUN018/RUN019 的范围纠正

网页端明确：正式 Qf 只应作用于凹栅水平槽底、Al2O3 直接接触 UID BetaGa2O3 的有效沟道界面；必须排除凹槽侧壁、左右 n 型 access 顶面、SiO2/BetaGa2O3、金属/介质和非栅控钝化界面。

RUN018/RUN019 的窗口覆盖槽底、侧壁和栅头下台面，因此只能保留为宽窗口灵敏度诊断，不能作为正式 Qf 拟合点。RUN020 从 RUN012 重新开始，不从 RUN018/019 继承 Qf 定义。

## RUN020 唯一变量

```text
interface qf=-1.0e12 x.min=4.5001 x.max=6.4999 y.min=0.129 y.max=0.131
```

槽底位于 `y=0.13 μm`、横向凹槽为 `x=4.5..6.5 μm`。x 边界略微内缩，避免把 `x=4.5/6.5 μm` 的垂直侧壁选入。Gate work function 保持 `5.78 eV`；其他物理与求解均冻结。

## 正式路径与状态

- 远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN020_RECESS_BOTTOM_QF_NEG1E12/`
- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN020_RECESS_BOTTOM_QF_NEG1E12/YUE2026_RUN020_RECESS_BOTTOM_QF_NEG1E12.in`
- 最新 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN020_RECESS_BOTTOM_QF_NEG1E12/YUE2026_RUN020_RECESS_BOTTOM_QF_NEG1E12_vd1_vg12.str`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN020_RECESS_BOTTOM_QF_NEG1E12/YUE2026_RUN020_RECESS_BOTTOM_QF_NEG1E12.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN020_RECESS_BOTTOM_QF_NEG1E12/YUE2026_RUN020_RECESS_BOTTOM_QF_NEG1E12.typescript`
- tmux：`yue_run020_bottomqf1e12_20260818`，已结束
- 进程：DeckBuild 与 `atlas.exe2` 均已退出
- 正式点数：121；最后实际偏压 `VGS=12.0 V`
- ATLAS 正常结束；计算段 `Error(s)=0`、`Warning(s)=0`
- 最大绝对 KCL：约 `1.00e-14 A`

## RUN012 与 RUN020 对比

| 指标 | RUN012，无 Qf | RUN020，槽底 Qf=-1e12 | 变化 |
|---|---:|---:|---:|
| 1 μA 交点 | 1.906983673 V | 2.307501809 V | +0.400518136 V |
| 2 μA 交点 | 1.953403620 V | 2.352991033 V | +0.399587413 V |
| 20 μA 阈值 | 3.064424661 V | 3.348992535 V | +0.284567874 V |
| ID@3.1 V | 20.081293 μA | 19.209973 μA | -4.339% |
| ID@9 V | 23.119390 μA | 23.098352 μA | -0.091% |
| ID@12 V | 23.470911 μA | 23.460009 μA | -0.0464% |
| SS，1e-12..1e-8 A | 73.2966 mV/dec | 73.2778 mV/dec | -0.026% |
| SS，1e-10..1e-6 A | 81.4829 mV/dec | 81.0656 mV/dec | -0.512% |

RUN020 在 1 pA 以上无非单调、肩峰或平台。ΔVTH 落入网页端 `+0.25..+0.55 V` 通过窗口，ID12 高于 21 μA 且几乎无损，SS 变化远小于 10%，完整收敛到 12 V，KCL 也满足门槛，因此本轮通过。

## 下一步

网页端此前锁定：若 `-1e12 cm^-2` 响应方向正确、曲线质量保持但幅度不足，则下一独立单点可做同一槽底界面的 `Qf=-2e12 cm^-2` 并检查近线性。下一轮仍必须从 RUN012 出发，仅改变 Qf；不得继承 RUN018/019 的宽界面窗口。
