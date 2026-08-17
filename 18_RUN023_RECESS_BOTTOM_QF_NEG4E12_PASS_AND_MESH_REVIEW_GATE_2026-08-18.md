# RUN023 槽底 Qf=-4e12 边界点与网格复核门

- 日期：2026-08-18
- 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`
- 材料身份：`BetaGa2O3`
- 基线：无固定电荷的 RUN012
- 阶段：正向 `VDS=1 V` 转移拟合；未运行 BV 或 SEB

## 唯一变量

```text
interface qf=-4.0e12 x.min=4.5001 x.max=6.4999 y.min=0.129 y.max=0.131
```

Qf 只在凹栅水平槽底 Al2O3/UID BetaGa2O3 界面生效。侧壁、access 顶面、SiO2 与其他界面均排除；Gate work function 保持 `5.78 eV`，其他几何、网格和物理全部冻结。

## 正式路径与状态

- 远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN023_RECESS_BOTTOM_QF_NEG4E12/`
- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN023_RECESS_BOTTOM_QF_NEG4E12/YUE2026_RUN023_RECESS_BOTTOM_QF_NEG4E12.in`
- 最新 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN023_RECESS_BOTTOM_QF_NEG4E12/YUE2026_RUN023_RECESS_BOTTOM_QF_NEG4E12_vd1_vg12.str`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN023_RECESS_BOTTOM_QF_NEG4E12/YUE2026_RUN023_RECESS_BOTTOM_QF_NEG4E12.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN023_RECESS_BOTTOM_QF_NEG4E12/YUE2026_RUN023_RECESS_BOTTOM_QF_NEG4E12.typescript`
- tmux：`yue_run023_bottomqf4e12_20260818`，已结束
- 进程：DeckBuild 与 `atlas.exe2` 均已退出
- 正式点数：121；最后实际偏压 `VGS=12.0 V`
- ATLAS 正常结束；计算段 `Error(s)=0`、`Warning(s)=0`
- 最大绝对 KCL：约 `1.00e-14 A`
- 正式目录已清理，只保留 `.in`、`.str`、`.log`、`.typescript`

## Qf 响应结果

| 指标 | RUN012，Qf=0 | RUN020，-1e12 | RUN021，-2e12 | RUN022，-3e12 | RUN023，-4e12 |
|---|---:|---:|---:|---:|---:|
| 1 μA 交点 | 1.906984 | 2.307502 | 2.708206 | 3.109047 | 3.509987 V |
| 2 μA 交点 | 1.953404 | 2.352991 | 2.753117 | 3.153636 | 3.554444 V |
| 20 μA 阈值 | 3.064425 | 3.348993 | 3.660352 | 3.990436 | 4.338700 V |
| ID@12 V | 23.470911 | 23.460009 | 23.448378 | 23.435929 | 23.422559 μA |
| SS，1e-12..1e-8 A | 73.2966 | 73.2778 | 73.2615 | 73.2472 | 73.2347 mV/dec |
| SS，1e-10..1e-6 A | 81.4829 | 81.0656 | 80.6886 | 80.3435 | 80.0232 mV/dec |

RUN023 的 1 μA/2 μA 位移相对 RUN020 单位电荷位移的倍率为 `4.002325/4.006734`，通过网页端预锁的 `3.8..4.2` 线性窗口，且两者没有分叉。20 μA 阈值位移倍率为 `4.477931`，仍处于允许的 `3.4..4.6` 边界窗口；RUN022 到 RUN023 的新增位移没有突然放大。ID@12 V 相对 RUN012 只下降 `0.2060%`，SS 保持稳定，曲线在 1 pA 以上无非单调。

因此 RUN023 通过，但它是 Qf 递进的最后一个边界诊断点。不得自动继续到 `-5e12 cm^-2`，也暂不组合功函数或 Dit。

## 网格复核门

当前 DevEdit 网格为 `2402` 个点、`4579` 个三角形，报告 `0` 个钝角三角形；但用户截图显示长距离 access/衬底与若干材料转角存在细长、高纵横比三角形，网格过渡也不均匀。这个视觉问题尚不能仅凭图片判定已经污染低压 Id–Vg，但在进入高场/BV/SEB 前必须做只改变网格生成器的独立性对照。

用户要求先由网页端联网复核适用于本结构和 Silvaco 2024 的 Victory Mesh conformal 方案。网页审核通过前不执行 Victory Mesh。候选方案必须保持几何、材料、掺杂、电极、Qf 与物理模型不变，不在高场边缘新增局部加密，并避免历史上同材料整界面 refinement 导致内存与交换抖动、无法返回提示符的失败路径。
