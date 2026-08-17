# RUN031–RUN034 标准 Conformal 金属 contact offset：硬停止

用户明确授权了一个独立的标准 Conformal mesh-only 对照。所有新增层都以 Gold/Nickel 金属 contact 与相邻非金属的真实接触面为共同几何起点；所谓双侧只表示同一接触面的两个接收侧。没有金属参与的 Al2O3/BetaGa2O3、SiO2/BetaGa2O3 等普通材料界面没有独立细化资格。本组运行没有使用 `SIMPLEX.MINIMAL`、whole-region `MAX.SIZE`、interface-distance、box、window、corner 或电场驱动选择，也没有运行 Id-Vg、BV 或 SEB。

RUN031 将 Gold 和 Nickel 同时写入 `INTERFACE.REGIONS`，VictoryMesh 在生成网格前以 `interface regions must be a single group` 停止。RUN032 按材料拆行，但 `MATERIAL:gold` 仍命中 drain 与 source/source_fp 多个独立电极 region，因此在生成网格前被同一检查停止。

RUN033 改用唯一电极名称。`NAME:source_fp` 对 `USER:source_nplus` 的第一次 offset 成功；紧接着对同一 `source_fp` 再执行 passivation offset 时失败，说明第一次 offset 已将该接收 region 拆成多个 group，不能再次作为单一接收组。本轮同样没有生成 vmesh STR。

RUN034 对每个接收 region 只调用一次：source/source_fp、drain、gate 各自把所有真实邻接非金属写入同一条 `OTHER.INTERFACE.REGIONS`；镜像方向也让 source n+、drain n+、gate oxide 和 passivation 各自只接收一次。所有命令统一使用 `DELTA=0.005 µm`、`NUM.INTERVALS=2`、`MULTIPLIER=1`，即从同一金属接触面向每个接收侧插入约5 nm和10 nm两层。

RUN034 成功完成 VictoryMesh 并保存 STR，随后被 ATLAS 成功载入。正式统计为3200 nodes、6173 triangles、786个钝角，即12.7329%。点数远低于25000，但钝角比例越过5%硬停止线。VictoryMesh 把原逻辑结构拆成29个 region 子区和9个 electrode 子区；电极名称与 SDB 仍归并为 drain、gate、source_fp 三类，ATLAS 的电极几何表也仍显示这三个名称，但载入记录明确是 `Read 29 regions` 和 `Read 9 electrodes`，不满足预锁的11区域/3电极结构不变量。

VictoryMesh 峰值驻留内存约165800 KiB，ATLAS 载入阶段约212852 KiB，swap 为0。不存在 Command Error、Cannot trap 或 did not converge；本轮只做网格和载入检查，没有电学求解，因此最后偏压为 N/A。

RUN034 远端目录：

`/root/YUE2026_EMODE_GRADED_DOPING/RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_MESHONLY/`

最新文件：

- `/root/YUE2026_EMODE_GRADED_DOPING/RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_MESHONLY/YUE2026_RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_MESHONLY.in`
- `/root/YUE2026_EMODE_GRADED_DOPING/RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_MESHONLY/YUE2026_RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_devedit_raw.str`
- `/root/YUE2026_EMODE_GRADED_DOPING/RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_MESHONLY/YUE2026_RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_vmesh.str`
- `/root/YUE2026_EMODE_GRADED_DOPING/RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_MESHONLY/YUE2026_RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_MESHONLY.log`
- `/root/YUE2026_EMODE_GRADED_DOPING/RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_MESHONLY/YUE2026_RUN034_STANDARD_CONFORMAL_CONTACT_GROUPED_OFFSET_MESHONLY.typescript`

RUN034 的 tmux 会话名为 `yue_run034_contact_offset`，启动时 pane PID 为84489、DeckBuild PID为84509。运行已正常结束，会话已退出，当前没有 DeckBuild、VictoryMesh、ATLAS 或 xinternal 计算进程。空的 `deckbuild_log.txt` 已删除，RUN031–RUN034 各目录只保留 `.in`、`.str`、`.log` 和 `.typescript`。

本轮因钝角和区域/电极子区数同时触发硬停止，没有运行 Id-Vg。结果已经提交网页端审核；在得到新的明确裁决前，不增加第二种网格机制，也不将 RUN034 作为正式物理基线。正式正向物理基线仍是 RUN023 DevEdit。
