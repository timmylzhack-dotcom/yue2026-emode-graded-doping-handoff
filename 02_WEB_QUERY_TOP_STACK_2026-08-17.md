# 网页端复核请求：按沉积顺序重建上层 SiO₂、Gate FP 与 Source FP

请结合此前共享对话中已经读取的论文 Fig. 1 工艺顺序和 Fig. 4(b) 右上电场剖面，只审查下面的公开文件，不读取论文 PDF，也不要从零重写器件：

- [当前公开交接](https://github.com/timmylzhack-dotcom/yue2026-emode-graded-doping-handoff/blob/main/00_HANDOFF_PUBLIC.md)
- [上一轮网页端复核](https://github.com/timmylzhack-dotcom/yue2026-emode-graded-doping-handoff/blob/main/01_WEB_REVIEW_2026-08-17.md)
- [候选拟合指标 Markdown](https://github.com/timmylzhack-dotcom/yue2026-emode-graded-doping-handoff/blob/main/Yue_2026_Ga2O3_RG_MOSFET_Silvaco_Fit_Criteria.md)
- [当前待修改 IN](https://github.com/timmylzhack-dotcom/yue2026-emode-graded-doping-handoff/blob/main/341b1ecd-715f-41c8-a0b7-cd1279053f27.in)

## 用户原问题

“这个栅极与源极的金属场板，以及介质层二氧化硅是淀积上去的。现在我需要你按照论文 Markdown 的要求，把当前代码生成的上层结构改成和论文图中右上角剖面一样的上层结构，给 Codex 可执行的修改指令；这个代码还有什么问题也指出来。”

用户所说的“图1右上角”应理解为此前共享对话中论文 Fig. 4(b) 的右上电场剖面所显示的层叠拓扑；“图2”是当前 IN 生成的结构。若这个映射不成立，请先指出歧义，但仍按论文工艺顺序审查。

## 必须服从的边界

- 材料身份必须是 `BetaGa2O3`，不能把器件当成 GaN 或普通未区分相的 Ga2O3。
- 以现有 DevEdit 和物理模型为母版做最小修改，不从零重写。
- 不在栅边缘、场板边缘、凹槽拐角或其他电场边缘敏感位置新增局部网格加密。候选拟合指标 Markdown 中要求边缘加密的段落与用户冻结约束冲突，不能执行。
- 正式运行目录只保留 `.in`、`.str`、`.log`、`.typescript`。候选指标文件建议输出 CSV/TXT/额外 Markdown 的内容不能直接执行。
- 不做参数扫描，不为了拟合修改 UID、功函数、界面电荷、陷阱或冲击电离参数。
- 小周长主基线采用 `Wch=20 µm`；请核对 ATLAS/DevEdit 中正确的二维宽度语法，但不要把未经手册核证的语法写成确定事实。

## 已冻结的几何与工艺

- 正 y 方向向下。
- Source 右边界 `x=1.0 µm`；Gate foot `x=2.5–4.5 µm`；Drain 左边界 `x=14.5 µm`。
- Gate head/Gate-connected FP `x=2.0–5.0 µm`，总宽3.0 µm，相对 Gate foot 左右各外伸0.5 µm。
- n+ 顶面为 `y=0`；访问区 n 层顶面为 `y=0.03 µm`；UID 顶面/凹槽底部为 `y=0.13 µm`。
- Al₂O₃ 厚20 nm：访问区为 `y=0.01–0.03 µm`，栅槽底为 `y=0.11–0.13 µm`。
- Source/Drain/Source FP 为 Ti/Au=`60/180 nm`；Gate/Gate FP 为 Ni/Au=`50/150 nm`。
- SiO₂ 厚300 nm，工艺顺序是先形成 Gate/Gate FP，再沉积 SiO₂，开孔后形成 Source FP。因此 Source FP 必须在 SiO₂ 上方，且与下方 Gate/Gate FP 由 SiO₂ 电隔离；Source FP 通过开孔/金属连接与 Source 等电位。
- Source FP 向漏端延伸3.5 µm。若按 Source 右边界起算，候选终点为 `x=4.5 µm`，但请把“论文直接给出3.5 µm”和“起点取 Source 右边界”严格区分；后者仍需从论文图确认。

## 当前 IN 已确认的问题

1. `region id=12`、`id=14`、`id=15` 都是只有两个点的零面积线区域，违反“电极必须有厚度”的要求。
2. Gate head 仍写成 `x=2.5–5.5 µm`，应改为已经冻结的 `x=2.0–5.0 µm`。
3. Source/Drain 目前是从 `y=-0.29` 到 `y=0` 的单一 Titanium 区域，总高度290 nm，不等于 Ti/Au=`60/180 nm` 的240 nm 分层金属。
4. Source FP 目前只是位于 `y=-0.29` 的 Titanium 线，没有 Ti/Au 分层、厚度、台阶或与 Source 的过孔连接。
5. Gate/Gate FP 目前只是 Nickel 线，没有 Ni/Au 分层厚度。
6. 当前 SiO₂ polygon 在没有实体 Gate 金属的情况下直接填入凹槽上方；它没有表达“Gate/Gate FP 先形成，SiO₂ 后沉积并包覆/跨越其上方，Source FP 最后位于 SiO₂ 上”的工艺拓扑。
7. `work.area y1=-0.31` 很可能不足以容纳 Gate 金属、300 nm SiO₂ 以及顶部240 nm Source FP 的完整厚度。
8. 当前 IN 没有明确的小器件二维宽度设置，需要核对正确语法和输出电流单位。

## 请重点判定的候选纵向坐标

下面只是供核对的几何候选，不是要求照抄：

- Source/Drain：Ti 从 `y=-0.06` 到 `0`，Au 从 `y=-0.24` 到 `-0.06`。
- Gate foot：接触 Al₂O₃ 的 Ni 从 `y=0.06` 到 `0.11`，其上的 Au 从 `y=-0.09` 到 `0.06`，总厚200 nm。
- 若300 nm SiO₂按共形沉积近似，则访问区 Al₂O₃ 顶面 `y=0.01` 上方的 SiO₂ 外表面约为 `y=-0.29`，Gate head 顶面 `y=-0.09` 上方的 SiO₂ 外表面约为 `y=-0.39`，因此 Source FP 可能需要用带台阶的有面积 polygon，而不是一条水平线。

请判断这个候选是否会错误地把“3 µm Gate head”只放在 Au 层、错误处理 Ni/Au 的共同版图轮廓，或者错误理解 PECVD SiO₂ 的共形/平坦化近似。论文没有给出的细节必须标为二维工程近似，不能写成论文值。

## 需要网页端给 Codex 的输出

请给出一份可执行但不从零重写的修改备忘录，内容包括：

1. 按真实沉积顺序列出从半导体表面向上的层次和电极归属，明确 Gate、Gate FP、SiO₂、Source FP 之间的上下关系。
2. 给出推荐的 DevEdit 非重叠区域拆分方案：每个区域的材料、`elec.id`、横纵坐标或 polygon 转折点；论文不确定处必须标成“二维近似/待确认”。
3. 明确 Source FP 如何跨越 Gate/Gate FP 上方的 SiO₂，并如何通过开孔与 Source 连通；不能用同一片金属把 Source FP 与 Gate FP 短接。
4. 指出 `work.area`、SiO₂ polygon、金属层、区域编号、网格约束和 `MESH WIDTH` 相关的最小代码修改。
5. 审查整个 IN 的其他问题，按“确定错误、潜在风险、当前基线可暂留”分类；不要把低压基线尚未包含完整 BV 扫描误判成错误。
6. 审查候选拟合指标 Markdown 的证据标记：特别核验“单指约500 µm [P]”是否真有论文正文/图注明确证据，并指出与用户冻结约束冲突的建议。

回答时请先给 Codex 一张简洁的区域/坐标表，再给修改顺序和代码审查。不要直接生成整份替代 IN，不要新增高场边缘局部网格加密，也不要建议调整物理参数。
