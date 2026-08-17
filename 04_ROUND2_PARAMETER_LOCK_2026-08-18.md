# Yue 2026 RG-MOSFET 第二轮参数锁定

本文件是公开脱敏的第二轮代码输入。参数只来自用户提供的英文论文 Markdown 的 Section II；不包含论文 PDF、SSH 信息、私钥、本地绝对路径或未公开物理模型。代码与结构截图分别见：

- [`YUE2026_RUN010_WEBPRO_PROCESS.in`](YUE2026_RUN010_WEBPRO_PROCESS.in)
- [`YUE2026_RUN010_WEBPRO_PROCESS_mesh.png`](YUE2026_RUN010_WEBPRO_PROCESS_mesh.png)

## 1. 必须先区分的厚度

| 对象 | 论文明确值 | 第二轮二维表达 | 证据边界 |
|---|---:|---|---|
| ALD Al2O3 | 20 nm | `0.02 um`，一次连续 `DEPOSIT` | 不能用多个 polygon 拼接 |
| Gate / Gate FP | Ni/Au 50/150 nm，总厚度 200 nm | 首版单一 Nickel conductor，`0.20 um` | 200 nm 是栅金属，不是 SiO2 |
| PECVD SiO2 | 300 nm | `0.30 um`，一次连续 `DEPOSIT` | **300 nm 是 SiO2 钝化层厚度，不是本文给出的金属厚度** |
| Source / Drain | Ti/Au 60/180 nm，总厚度 240 nm | 首版单一 Gold conductor，`0.24 um` | 240 nm 是源漏金属总厚度 |
| Source FP | Ti/Au 60/180 nm，总厚度 240 nm | 独立区域名 `source_fp`，`0.24 um`，当前 `elec.id=1` | 不与 Source 区域强制 `AUTOMATIC.JOIN`；电气上仍与 Source 等电位 |
| 大周长器件 metal thickening | 论文未给最终厚度 | 不得填成文献值 | 若采用 300 nm 金属，只能标为用户覆盖值/待验证 |

## 2. 外延、掺杂和横向尺寸

器件材料必须是 `BetaGa2O3`。外延自下而上为 UID 100 nm、n 层 100 nm、n+ 层 30 nm；n 层为 `1e18 cm^-3`，n+ 层为 `7e18 cm^-3`。栅下 100 nm n 层完全移除并保留 UID，访问区保留 n 层。

论文尺寸为 `LGS/LG/LGD = 1.5/2.0/10.0 um`，Gate head 为 3.0 um，Source FP 从 Gate 漏侧边缘向 Drain 延伸 3.5 um。当前 Fig. 4 对齐坐标为：

| 边缘 | x (um) |
|---|---:|
| Source 区 | 0.0–3.0 |
| Source 内缘 | 3.0 |
| Gate foot | 4.5–6.5 |
| Gate head 投影 | 4.0–7.0 |
| Source FP 末端 | 10.0 |
| Drain 内缘 | 16.5 |
| Drain 区 | 16.5–19.5 |

## 3. 第二轮工艺顺序

顺序必须保持为：Source/Drain 金属 → 一次连续 Al2O3 → 一次局部 Gate 金属 → 一次连续 SiO2 → Source via 开口 → 一次局部 Source FP 金属。

只有 `via_cut` 可以使用临时 `REGION` 覆盖并删除。Al2O3、Gate、SiO2 与 Source FP 都必须由 `DEPOSIT` 形成，不能用手画 polygon 假装沉积。不得使用 `SIDE=TOP` 或 `ROUNDING.ANGLE`；本机 DevEdit 2.8.26 不接受这两个写法。不得加入高场边缘局部网格框，不进入 Id-Vg、BV 或 SEB。

## 4. RUN010 已验证事实和第二轮待修点

RUN010 在 DevEdit 2.8.26 中完成：0 Error、0 Warning、2402 points、4579 triangles、0 obtuse triangles。STR 中保留 `BetaGa2O3`、Gold、Nickel、`gate` (`elec.id=3`)、`drain` (`elec.id=2`) 与独立 `source_fp` (`elec.id=1`)。

为避免 SiO2 在器件左右外边界触发 DevEdit 几何内核退出，当前 SiO2 窗口使用 `start=0.40 end=19.49`；Source via 为 `x=0.40..1.60`。这消除了 Source 最左侧侵入金属的小块 SiO2，但右端 `19.49` 相对器件边界 `19.50` 仍是 0.01 um 的数值规避量，必须在第二轮 STR 中检查 Drain 外缘包覆是否可接受。

第二轮代码应针对所附真实截图修正沉积窗口和 via，但不得改变上述论文厚度、外延、掺杂、Gate/Source FP 长度或材料身份。输出仍应标为“候选代码，待真实 STR 验证”。

