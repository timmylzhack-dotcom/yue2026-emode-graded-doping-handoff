# RUN017 栅有效功函数单因素验收

- 日期：2026-08-18
- 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`
- 材料身份：`BetaGa2O3`
- 阶段：正向 `VDS=1 V` 转移拟合；未运行 BV 或 SEB
- 母版：RUN012

## 唯一变量

```text
contact name=gate workfunc=5.78
→
contact name=gate workfunc=5.88
```

结构、网格、BetaGa2O3 材料卡、掺杂、UID 动态陷阱缺省状态、基底陷阱、固定界面电荷、Dit、迁移率和求解器全部冻结。ATLAS 接触表实际显示 gate work function 为 `5.880 eV`。

## 正式路径与状态

- 远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN017_GATE_WF_5P88/`
- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN017_GATE_WF_5P88/YUE2026_RUN017_GATE_WF_5P88.in`
- 最新 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN017_GATE_WF_5P88/YUE2026_RUN017_GATE_WF_5P88_vd1_vg12.str`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN017_GATE_WF_5P88/YUE2026_RUN017_GATE_WF_5P88.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN017_GATE_WF_5P88/YUE2026_RUN017_GATE_WF_5P88.typescript`
- tmux：`yue_run017_gatewf5p88_20260818`，已结束
- 进程：DeckBuild 与 `atlas.exe2` 均已退出
- 正式点数：121
- 最后实际偏压：`VGS=12.0 V`
- ATLAS：正常结束，计算段 `Error(s)=0`、`Warning(s)=0`

## RUN012 与 RUN017 对比

| 指标 | RUN012，5.78 eV | RUN017，5.88 eV | 变化 |
|---|---:|---:|---:|
| 1 μA 交点 | 1.906983673 V | 2.006983673 V | +0.100000 V |
| 2 μA 交点 | 1.953403620 V | 2.053403620 V | +0.100000 V |
| 20 μA 阈值 | 3.064424661 V | 3.164424661 V | +0.100000 V |
| ID@3.1 V | 20.08129257 μA | 19.85278436 μA | -1.1379% |
| ID@9 V | 23.11939045 μA | 23.10393288 μA | -0.0669% |
| ID@12 V | 23.47091106 μA | 23.46177676 μA | -0.0389% |
| SS，1e-12..1e-8 A | 73.2966 mV/dec | 73.2966 mV/dec | 0 |
| SS，1e-10..1e-6 A | 81.4829 mV/dec | 81.4829 mV/dec | 0 |
| 最大绝对 KCL | 4.496e-17 A | 4.540e-17 A | 无实质退化 |

RUN017 在 1 pA 以上没有非单调点。12 V 电流高于网页端预锁的 20 μA 下限，且相对下降远小于 15%；阈值增量落在 `+0.05..+0.15 V` 通过窗口正中，SS 和曲线形状保持不变，并完整收敛到 12 V。因此本轮通过。

## 物理含义与边界

栅有效功函数对当前模型给出近似一比一的阈值平移，说明 Gate 接触对象和功函数施加方式正确。它是干净、可辨识的有限精调旋钮，但 `+0.10 eV` 只能解释 `+0.10 V`，不能用不可信的大幅功函数改动承担距离论文约 9 V 的剩余约 5.84 V。

下一大方向交由网页端审核均匀负固定界面电荷。审核返回前，不继续把功函数推至 5.98/6.08 eV，不恢复 UID 动态深受主，不恢复 `Acceptors=5e17 cm^-3` 静态补偿，也不通过增加 `MAXTRAPS/ITLIMIT` 强行推进。
