# RUN015/RUN016 等效静态 UID 补偿边界

- 日期：2026-08-18
- 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`
- 器件材料身份：`BetaGa2O3`
- 阶段：只做正向 `VDS=1 V` 转移测试；未进入 BV 或 SEB
- 母版：RUN012；保留已确认的淀积结构、材料卡、接触、基底陷阱和正向扫描框架

## 本轮唯一物理变量

RUN015 关闭 UID 动态深受主，在 `uid_channel` 中加入 DevEdit 静态 `Acceptors=5.0e17 cm^-3`。这只是等效静电补偿代理，不是论文报告的真实受主浓度，也不能写回文献参数表作为实验事实。

RUN016 不改变任何物理参数，只对 RUN015 做一次数值收敛救援：

```text
method newton trap maxtraps=10 climit=1e-4 itlimit=50
→
method newton trap maxtraps=30 climit=1e-4 itlimit=80
```

UID 区未重新加入动态 `TRAP`；原有基底 `region=6` 陷阱保持不变。未改变几何、网格、BetaGa2O3 材料参数、栅功函数、固定电荷、Dit 或迁移率。

## RUN015 正式结果

- 远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN015_UID_STATIC_ACCEPTOR_5E17/`
- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN015_UID_STATIC_ACCEPTOR_5E17/YUE2026_RUN015_UID_STATIC_ACCEPTOR_5E17.in`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN015_UID_STATIC_ACCEPTOR_5E17/YUE2026_RUN015_UID_STATIC_ACCEPTOR_5E17.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN015_UID_STATIC_ACCEPTOR_5E17/YUE2026_RUN015_UID_STATIC_ACCEPTOR_5E17.typescript`
- 正式收敛点：50
- 最后实际偏压：`VGS=4.5 V`
- 最后 `ID=-2.901328900e-18 A`
- `ID@3.1 V=-2.032311815e-17 A`
- 全程最大 `|ID|=6.876149493e-17 A`
- 最大绝对 KCL 残差：`6.6550095134e-17 A`

RUN015 在 4.5 V 以上发生偏压回退并失败。没有达到 9 V 或 12 V；不得依据后续 `save` 语句或目标文件名判断成功。

## RUN016 唯一收敛救援结果

- 远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN016_UID_STATIC_ACCEPTOR_5E17_CONV/`
- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN016_UID_STATIC_ACCEPTOR_5E17_CONV/YUE2026_RUN016_UID_STATIC_ACCEPTOR_5E17_CONV.in`
- 最新实际 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN016_UID_STATIC_ACCEPTOR_5E17_CONV/YUE2026_RUN016_UID_STATIC_ACCEPTOR_5E17_CONV_vd1_vg3p1.str`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN016_UID_STATIC_ACCEPTOR_5E17_CONV/YUE2026_RUN016_UID_STATIC_ACCEPTOR_5E17_CONV.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN016_UID_STATIC_ACCEPTOR_5E17_CONV/YUE2026_RUN016_UID_STATIC_ACCEPTOR_5E17_CONV.typescript`
- tmux：`yue_run016_uidstatic5e17_conv_20260818`，已结束
- 进程：DeckBuild 与 `atlas.exe2` 均已退出
- 正式收敛点：145
- 最后实际偏压：`VGS=4.541605936 V`
- 最后 `ID=-2.006061697e-17 A`
- `ID@3.1 V=-2.032311815e-17 A`
- `ID@4.5 V=-2.901328900e-18 A`
- 全程最大 `|ID|=7.511678681e-17 A`
- 最大绝对 KCL 残差：`7.6665025714e-17 A`
- 终止信息：`Cannot trap. Cannot reduce bias step`，ATLAS exit code 1

RUN016 在临界区反复缩小偏压步长，最终增量退化到约 `1e-9 V`，只比 RUN015 多推进约 0.0416 V。最新保存 STR 的文件内部状态已核验为约 `VGS=3.100000 V`；4.541605936 V 处没有保存 STR。

## 判定与下一步

`Acceptors=5.0e17 cm^-3` 的等效静态 UID 补偿在当前冻结的 `incomplete`/BetaGa2O3 模型下既使器件在低压范围完全关闭，又在约 4.54 V 形成不可跨越的数值或物理突变。增加回退次数和迭代上限未解决问题，因此本路线在该浓度停止，不再追加 Gummel、额外步长技巧或其他物理参数。

下一步最小单因素方案是回到能完整扫至 12 V 的 RUN012，保持 UID 动态陷阱关闭、静态受主关闭，只测试栅有效功函数的小幅正增量。建议先做 `+0.1 eV` 单点；固定界面电荷和 Dit 后置。仍应先完成正向拟合，不运行 BV 或 SEB。
