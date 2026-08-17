# RUN022 槽底 Qf=-3e12 响应边界

- 日期：2026-08-18
- 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`
- 材料身份：`BetaGa2O3`
- 基线：无固定电荷的 RUN012
- 阶段：正向 `VDS=1 V` 转移拟合；未运行 BV 或 SEB

## 唯一变量

```text
interface qf=-3.0e12 x.min=4.5001 x.max=6.4999 y.min=0.129 y.max=0.131
```

Qf 只在凹栅水平槽底 Al2O3/UID BetaGa2O3 界面生效，输入中只有一条 Qf。侧壁、access 顶面、SiO2 与其他界面均排除；Gate work function 保持 `5.78 eV`，其他物理全部冻结。

## 正式路径与状态

- 远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN022_RECESS_BOTTOM_QF_NEG3E12/`
- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN022_RECESS_BOTTOM_QF_NEG3E12/YUE2026_RUN022_RECESS_BOTTOM_QF_NEG3E12.in`
- 最新 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN022_RECESS_BOTTOM_QF_NEG3E12/YUE2026_RUN022_RECESS_BOTTOM_QF_NEG3E12_vd1_vg12.str`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN022_RECESS_BOTTOM_QF_NEG3E12/YUE2026_RUN022_RECESS_BOTTOM_QF_NEG3E12.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN022_RECESS_BOTTOM_QF_NEG3E12/YUE2026_RUN022_RECESS_BOTTOM_QF_NEG3E12.typescript`
- tmux：`yue_run022_bottomqf3e12_20260818`，已结束
- 进程：DeckBuild 与 `atlas.exe2` 均已退出
- 正式点数：121；最后实际偏压 `VGS=12.0 V`
- ATLAS 正常结束；计算段 `Error(s)=0`、`Warning(s)=0`
- 最大绝对 KCL：约 `1.00e-14 A`

## 结果与线性检查

| 指标 | RUN012，Qf=0 | RUN020，-1e12 | RUN021，-2e12 | RUN022，-3e12 |
|---|---:|---:|---:|---:|
| 1 μA 交点 | 1.906984 | 2.307502 | 2.708206 | 3.109047 V |
| 2 μA 交点 | 1.953404 | 2.352991 | 2.753117 | 3.153636 V |
| 20 μA 阈值 | 3.064425 | 3.348993 | 3.660352 | 3.990436 V |
| ID@12 V | 23.470911 | 23.460009 | 23.448378 | 23.435929 μA |
| SS，低窗口 | 73.2966 | 73.2778 | 73.2615 | 73.2472 mV/dec |
| SS，宽窗口 | 81.4829 | 81.0656 | 80.6886 | 80.3435 mV/dec |

RUN022 的 20 μA 阈值相对 RUN012 正移 `0.926011696 V`，是 RUN020 位移的 `3.254098` 倍；比理想三倍高约 8.5%。1 μA/2 μA 位移是 RUN020 对应位移的 `3.001270/3.003680` 倍，低电流区仍接近严格线性。

ID12 只下降 `0.1490%`；两个 SS 相对 RUN012 变化约 `-0.0674%/-1.3984%`。曲线无肩峰、平台、锯齿或非单调，完整收敛和 KCL 均通过。

## 当前暂停点

RUN022 没有触发硬停止条件，但 20 μA 判据已出现轻微超线性。当前先暂停自动增加 Qf，等待网页端读取 RUN021/RUN022 后审核下一步。不能在没有复核的情况下连续推向接近 `1e13 cm^-2`，也不能同时叠加功函数、Dit 或其他陷阱。
