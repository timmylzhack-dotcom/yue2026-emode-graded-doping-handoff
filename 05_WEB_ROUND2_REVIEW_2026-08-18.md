# 网页端第二轮审查（2026-08-18）

本页保存网页端在读取公开脱敏参数锁定文档、RUN010 输入文件以及最新 TonyPlot 结构摘要后给出的第二轮审查意见。它是审查记录，不替代 STR 实测，也不表示已完成新的仿真。

## A. 参数核对

| 项目 | 锁定值 | 第二轮意见 |
|---|---:|---|
| Source / Drain 金属 | Gold 二维等效，0.24 um | 保留 |
| Al2O3 | 一次连续 `DEPOSIT`，0.02 um | 保留，不改为 polygon 拼接 |
| Gate / Gate FP | Nickel 二维等效，局部 `DEPOSIT`，0.20 um | 保留 |
| SiO2 | 一次连续 `DEPOSIT`，0.30 um | 保留；300 nm 属于 SiO2，不是金属厚度 |
| Source FP | Gold 二维等效，0.24 um；`region.name=source_fp`、`elec.id=1` | 保留，不加 `AUTOMATIC.JOIN` |
| Via | 临时 `REGION` 后 `REGION DELETE` | 保留为唯一 polygon 用途 |
| 材料身份 | BetaGa2O3 | 保留 |
| 工艺顺序 | S/D → Al2O3 → Gate → SiO2 → Via → Source FP | 保留 |

网页端特别指出，不应把 300 nm 误写成金属厚度，也不应把 Source FP 改回 `region.name=source`，或给 `source_fp` 增加 `AUTOMATIC.JOIN`。

## B. 相对 RUN010 的逐行意见

网页端本轮没有建议新增 `DEPOSIT`、删除工艺步骤或更换几何坐标。建议保留以下现有语句及语义：

- Source 与 Drain 的局部金属沉积；
- Al2O3 的连续沉积以及现有 `AUTOMATIC.JOIN`；
- Gate Nickel 的局部沉积以及现有 `AUTOMATIC.JOIN`；
- `SiO2 thickness=0.30 start=0.40 end=19.49 automatic.join`；
- `via_cut` 临时区域与对应的 `region delete id=90`；
- `region.name=source_fp elec.id=1` 且不带 `AUTOMATIC.JOIN` 的 Source FP 沉积。

因此，第二轮审查没有产出一份与 RUN010 不同的新 Stage-1 代码；它的实际修改意见是冻结已经通过 DevEdit 的 RUN010 几何，先按以下验收点复核 STR，再进入后续低压验证。

## C. 三个数值规避

`SiO2 START=0.40` 建议保留，因为它已经消除了 Source 金属内部残留的小块 SiO2，最新结构图未再出现该缺陷。

via 的横向范围 `0.40..1.60` 建议保留，因为当前 Source FP 已能通过该窗口与 Source 建立二维连接，且未见与 Gate 短接；在有相反 STR 证据前不再移动窗口。

`SiO2 END=19.49` 建议保留。`19.50` 已触发 DevEdit 几何内核异常，而 `19.49` 已得到 0 error、0 warning 的 STR，并仍能把 Drain 侧 SiO2 包覆延伸到接近器件右边界。该 0.01 um 差值记录为软件兼容规避，不解释为论文几何参数。

## D. TonyPlot 验收点

1. Al2O3 必须由一次连续沉积形成，沿左 access、凹槽侧壁、槽底及右 access 连续包覆，不能出现 polygon 拼接缝。
2. SiO2 应形成四个可辨认的凸起，依次对应 Source 台阶、Gate 左肩、Gate 右肩和 Drain 台阶；Drain 外缘仍被 SiO2 包覆。
3. Source FP 应覆盖前三个 SiO2 凸起并越过 Gate 进入漂移区，在平坦漂移区终止，不延伸到 Drain 侧第四凸起。
4. Source FP 只通过 via 与 Source 连通；via 不切入 Source 金属以下，也不接触 Gate。
5. Gate 应由一次局部沉积沿 Al2O3 拓扑生成，不得改回矩形或多个 polygon 拼接。
6. 最终外部端子只有 source、drain、gate；`source_fp` 保持独立区域名并使用 `elec.id=1`，不依赖 `AUTOMATIC.JOIN` 与 Source 强制合并。

## 当前证据

RUN010 最新 DevEdit 结果为 0 error、0 warning、2402 points、4579 triangles、0 obtuse。此记录只认可已生成 STR 的几何证据；尚未由新的 ATLAS 低压求解重新验证电极连通、RHS 和绝对 KCL。
