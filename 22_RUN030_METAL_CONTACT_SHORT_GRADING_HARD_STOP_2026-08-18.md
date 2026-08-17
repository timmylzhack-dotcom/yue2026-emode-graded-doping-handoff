# RUN030 金属 contact 短程双侧渐变：点数硬停止

RUN030 用于纠正 RUN029 的整条 Al2O3/BetaGa2O3 界面加密。它以 RUN027 的 VictoryMesh whole-region 规则为母版，只把 Gold/Nickel 金属 contact 与相邻非金属的真实界面作为种子；普通 Al2O3/BetaGa2O3、SiO2/BetaGa2O3 界面不单独作为种子。本轮只做 mesh-only，没有运行 Id-Vg、BV 或 SEB。

唯一新增的网格规则为：

```text
refine regions="MATERIAL:gold,MATERIAL:nickel,USER:gate_oxide,USER:passivation,USER:source_nplus,USER:drain_nplus,USER:access_left,USER:access_right,USER:uid_channel" \
       interface.regions="MATERIAL:gold,MATERIAL:nickel" \
       other.interface.regions="USER:gate_oxide,USER:passivation,USER:source_nplus,USER:drain_nplus,USER:access_left,USER:access_right,USER:uid_channel" \
       max.interface.size=0.010 \
       max.interface.size.distance=0.010 \
       max.interface.distance=0.025 \
       max.interface.distance.size=0.025 \
       grading=linear
```

该规则按网页端最终批准值执行：界面尺寸 10 nm，前 10 nm 保持细尺寸，在 25 nm 内线性过渡到 25 nm。VictoryMesh 发出警告：`max.interface.distance` 最好至少为 `max.interface.distance.size` 的三倍，才能形成平滑过渡。由于用户明确要求扩散距离很短，本轮没有擅自扩大到 75 nm。

实际结果：

- VictoryMesh：43963 points、87435 triangles。
- ATLAS 只读载入：43963 nodes、87435 triangles、11 regions、3 electrodes。
- Obtuse triangles：2967，比例 3.39338%。
- electrode 类型区域：28799 elements、15882 2D points。
- structure 类型区域：58636 elements、30834 2D points。
- VictoryMesh 峰值驻留内存约 609872 KiB；swap 为 0。
- Gold、Nickel 只与 Al2O3、BetaGa2O3、SiO2 相邻；日志没有显示普通非金属界面被单独选作种子。

预锁门槛是 points 优先不超过 25000，超过 25000 暂停复核，绝对不得超过 35000。RUN030 的 43963 points 已越过绝对硬停止线，因此没有运行 Id-Vg。虽然向接触两侧的扩散距离只有 25 nm，但完整 Gold/Nickel contact 轮廓的总长度仍使电极及相邻非金属产生大量节点。

远端正式目录：

`/root/YUE2026_EMODE_GRADED_DOPING/RUN030_VICTORYMESH_METAL_CONTACT_SHORT_GRADING_MESHONLY/`

保留文件：

- `/root/YUE2026_EMODE_GRADED_DOPING/RUN030_VICTORYMESH_METAL_CONTACT_SHORT_GRADING_MESHONLY/YUE2026_RUN030_VICTORYMESH_METAL_CONTACT_SHORT_GRADING_MESHONLY.in`
- `/root/YUE2026_EMODE_GRADED_DOPING/RUN030_VICTORYMESH_METAL_CONTACT_SHORT_GRADING_MESHONLY/YUE2026_RUN030_VICTORYMESH_METAL_CONTACT_SHORT_GRADING_devedit_raw.str`
- `/root/YUE2026_EMODE_GRADED_DOPING/RUN030_VICTORYMESH_METAL_CONTACT_SHORT_GRADING_MESHONLY/YUE2026_RUN030_VICTORYMESH_METAL_CONTACT_SHORT_GRADING_vmesh.str`
- `/root/YUE2026_EMODE_GRADED_DOPING/RUN030_VICTORYMESH_METAL_CONTACT_SHORT_GRADING_MESHONLY/YUE2026_RUN030_VICTORYMESH_METAL_CONTACT_SHORT_GRADING_MESHONLY.typescript`

自动生成的空 `deckbuild_log.txt` 已删除。tmux 会话已结束，当前没有 DeckBuild、VictoryMesh、ATLAS 或 xinternal 计算进程。mesh-only 不存在实际扫压，最后偏压记为 N/A。

在新的网页端批准前，不缩小单个高场 contact、不增加第二类 refinement，也不运行 Id-Vg。下一步只允许根据 RUN027/RUN030 的结构对照判断完整 contact 轮廓中哪些部分造成点数膨胀。
