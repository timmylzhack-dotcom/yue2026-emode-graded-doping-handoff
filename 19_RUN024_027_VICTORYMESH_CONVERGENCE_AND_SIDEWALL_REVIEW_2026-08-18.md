# RUN024–RUN027 VictoryMesh 收敛证据与 20 nm Al2O3 侧壁审核门

任务 Session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`

本记录只保存已经完成的网格与低压 Id–Vg 结果。任何新的界面加密、薄膜各向同性加密或其他网格方案，必须先把代码、现有对照和用户提出的风险交给网页端审核；网页端明确批准首轮单变量方案和停止条件之前，不得生成或运行 RUN028。

## 已完成运行

### RUN024：较粗的 VictoryMesh 网格

- 算法：`REMESH CONFORMAL SIMPLEX.MINIMAL`
- 规则：只有整区域 `MAX.SIZE`，无 interface、shape、box、edge 或 corner refinement
- 网格：5478 points、10574 triangles、405 obtuse triangles，obtuse 比例 3.83015%
- 资源：约 210 MB，swap 为 0
- 结果：区域、材料、掺杂场和三个电极均被保留

远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN024_VICTORYMESH_CONFORMAL_MINIMAL_MESHONLY/`

### RUN025：较粗 VictoryMesh 的 Id–Vg

- 实际完成到 VGS=12 V，无 Cannot trap、非单调或 KCL 异常
- VGS@1 μA：3.5160106218 V
- VGS@2 μA：3.5612807995 V
- VGS@20 μA：4.0217086964 V
- ID@12 V：27.05693563 μA
- SS：75.3929165 / 79.9779934 mV/dec

远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN025_VICTORYMESH_IDVG_QF_NEG4E12/`

### RUN026：较密的活跃区 VictoryMesh 网格

相对 RUN024，只把 n+、access、UID 和 Al2O3 的整区域 x/y `MAX.SIZE` 各缩小一半；SiO2、Gold、Nickel 和 Fe substrate 保持不变。没有加入任何界面或局部高场边缘规则。

- 网格：10848 points、21278 triangles、635 obtuse triangles，obtuse 比例 2.9843%
- 资源：约 267 MB，swap 为 0
- ATLAS 只读载入确认：11 个区域、3 个电极

远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN026_VICTORYMESH_DENSE_ACTIVE_MESHONLY/`

### RUN027：较密 VictoryMesh 的 Id–Vg

- 123 个数据点，实际完成到 VGS=12 V
- VGS@1 μA：3.5181644523 V
- VGS@2 μA：3.5651024024 V
- VGS@20 μA：3.9817824589 V
- ID@12 V：27.09629538 μA
- SS：75.7023325 / 80.4993103 mV/dec
- 最大绝对端子电流和约 1e-14 A；1 pA 以上无非单调

远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN027_VICTORYMESH_DENSE_IDVG_QF_NEG4E12/`

- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN027_VICTORYMESH_DENSE_IDVG_QF_NEG4E12/YUE2026_RUN027_VICTORYMESH_DENSE_IDVG_QF_NEG4E12.in`
- 最新 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN027_VICTORYMESH_DENSE_IDVG_QF_NEG4E12/YUE2026_RUN027_VICTORYMESH_DENSE_IDVG_QF_NEG4E12_vd1_vg12.str`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN027_VICTORYMESH_DENSE_IDVG_QF_NEG4E12/YUE2026_RUN027_VICTORYMESH_DENSE_IDVG_QF_NEG4E12.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN027_VICTORYMESH_DENSE_IDVG_QF_NEG4E12/YUE2026_RUN027_VICTORYMESH_DENSE_IDVG_QF_NEG4E12.typescript`

## 网格独立性判读

RUN027 相对 RUN025：VGS@20 μA 只变化 -0.03993 V，ID@12 V 只变化约 +0.145%，两个 SS 变化均小于 1%。因此这两档 VictoryMesh 已基本自收敛。

RUN027 相对原 DevEdit RUN023：VGS@20 μA 低 0.35692 V，ID@12 V 高约 15.69%；低电流交点只相差约 8–11 mV，SS 差异较小。现有证据说明 VictoryMesh 家族在低电流电静力上接近 DevEdit，但在高导通输运上收敛到不同结果。继续盲目缩小所有整区域尺寸不能解释或消除这一差异。

## 20 nm Al2O3 侧壁证据

构造命令为一次连续淀积：

```text
deposit material=Al2O3 thickness=0.02 start=3.00 end=16.50 region.id=83 region.name=gate_oxide automatic.join
deposit material=Nickel thickness=0.20 start=4.00 end=7.00 region.id=84 region.name=gate elec.id=3 automatic.join
```

对 RUN026 的 DevEdit 原始 ASCII STR 做只读坐标核查后，左侧凹槽侧壁 Al2O3 的两条边界位于 x=4.500 μm 和 x=4.520 μm，并从约 y=0.03 μm 连续延伸至 y=0.13 μm。原始几何法向厚度确为 0.020 μm，Nickel 淀积没有把这段 Al2O3 真正覆盖或删除。

当前 VictoryMesh 对 Al2O3 使用：

```text
refine regions="USER:gate_oxide" max.size="0.10,0.0050"
```

槽底薄膜的法向为 y，0.005 μm 能形成数层；侧壁薄膜的法向为 x，而 0.10 μm 大于整个 0.02 μm 膜厚，因此侧壁只剩约一层单元有直接的尺寸规则原因。截图中的“金属吞掉氧化层”主要是重网格离散和显示效果，不是原始 DevEdit 几何被吞。

## 网页端最终审核

用户提出应考虑类似 Sentaurus 的 interface refinement，沿不规则真实材料界面向两侧渐变，以改善薄膜法向层数和尺寸突变。网页端在读取局部 Mesh 图和原始 STR 坐标证据后，确认当前问题是“几何仍在，但侧壁法向解析不足”，并在下列两个互斥首轮变量中完成选择：

1. 只把 Al2O3 整区域改成更接近各向同性的 x/y `MAX.SIZE`；
2. 只对严格限定的真实异质材料界面使用 interface grading。

最终只批准方案 1。本轮保持 RUN027 的 `REMESH CONFORMAL SIMPLEX.MINIMAL`、`SPACING` 和其他所有区域规则不变，只把：

```text
refine regions="USER:gate_oxide" max.size="0.10,0.0050"
```

替换为：

```text
refine regions="USER:gate_oxide" max.size="0.010,0.005"
```

本轮不批准 interface refinement，不增加 Gold/Nickel 选择器，不加入 BetaGa2O3/BetaGa2O3 人为同材质边界，也不加入任何局部高场 box、shape、edge 或 corner 规则。

Mesh-only 要求两侧壁至少 2 个完整法向单元层，目标 3 层；继续只有 1 层即失败。预期点数约 15000–25000，超过 35000 硬停止；峰值 RAM 目标不超过 1 GiB，超过 2 GiB 硬停止；swap 必须为 0，持续增长或超过约 256 MiB 停止；obtuse 比例应不超过约 4%，超过 5% 停止。区域、材料、掺杂字段和三个端子必须保持。

若 mesh-only 通过，同物理 Id–Vg 必须完整到 12 V。相对 RUN027，1 μA 与 2 μA 交点差应不超过 0.02 V，SS 差不超过 3%；相对 RUN023 的完整高导通通过门槛仍为 VGS@20 μA 差不超过 0.10 V、ID@12 V 差不超过 3%、SS 差不超过 5%。若尚未完全通过，至少要求 VGS@20 μA 和 ID@12 V 与 RUN023 的差距同时缩小 20%，且低电流与 SS 稳定。若相对 RUN027 的 VGS@20 μA 变化不足 0.05 V且 ID@12 V 变化不足 2%，或者结果继续远离 RUN023，则停止该方案，只允许做相同显示比例下的侧壁厚度、层数、单元形状以及相同偏压电子浓度/电流密度的只读对照。
