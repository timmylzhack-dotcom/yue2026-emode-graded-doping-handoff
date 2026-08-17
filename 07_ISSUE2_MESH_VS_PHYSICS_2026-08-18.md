# Issue 2：RUN011 的 VTH≈3.06 V 更可能是网格问题还是未标定物理？

Codex 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`

请只做诊断排序和下一步最小验证建议，不要代写整份新器件代码，不要进入 BV/SEB，也不要改变论文锁定的几何、层厚、n/n+ 掺杂和场板尺寸。

## 已确认事实

- 器件材料是 BetaGa2O3，不是 GaN；`user.default=GaN` 只是 ATLAS 用户材料的数值模板。
- RUN010/RUN011 上层结构采用真实 DevEdit `DEPOSIT`：Al2O3 0.02 um、Gate 0.20 um、SiO2 0.30 um、S/D 与 Source FP 0.24 um。
- Source FP 区域名保持 `source_fp`，使用 `elec.id=1` 与 Source 等电位，不加 `AUTOMATIC.JOIN`。
- DevEdit 网格为 2402 points、4579 triangles、0 obtuse、0 error、0 warning；没有高场边缘局部加密 box。
- 导入 ATLAS 后已按真实映射把 BetaGa2O3 绑定到 `region=1..6`，Al2O3 绑定到 `region=8`，SiO2 绑定到 `region=10`。错误的旧区号运行在进入偏压前已停止，不属于正式结果。
- 正式条件：`Wch=20 um`、`VDS=1 V`、`VGS=0..12 V`。
- `VGS=12 V` 时 `ID=2.347091106e-5 A`，约 `1.1735 mA/mm`；最大绝对 KCL 约 `4.50e-17 A`。
- 按论文 `ID=1 mA/mm` 判据，20 um 器件对应 20 uA；RUN011 首次跨过该电流的采样点是 3.1 V，插值得 `VTH≈3.06 V`，而论文目标约为 9 V。
- 当前 UID 区施主浓度是继承/首版值 `5e15 cm^-3`；Ni 有效功函数继承为 5.78 eV；论文没有给出 UID 净补偿、有效功函数、Al2O3/UID 固定电荷或 Dit 的唯一数值。

当前 Mesh 图（RUN010 与 RUN011 几何相同）：

![RUN011 mesh](https://raw.githubusercontent.com/timmylzhack-dotcom/yue2026-emode-graded-doping-handoff/refs/heads/codex/round2-parameter-lock-20260818/YUE2026_RUN010_WEBPRO_PROCESS_mesh.png)

用户随后会在本 Issue 粘贴以下图片：

1. 栅槽局部 Mesh + Regions 放大图，约 `x=3..8 um`，显示 Gate、20 nm Al2O3、UID、左右 access 和网格线。
2. `VDS=1 V, VGS=0 V` 的对数 Electron Concentration 等高线。
3. `VDS=1 V, VGS=12 V` 的对数 Electron Concentration 等高线，与上一张同视窗、同色标。
4. 可选：上述两个状态的 Conduction Band 或 Potential 等高线，同视窗、同色标。

## 请回答

A. 在当前网格统计、KCL 和连续收敛都正常的前提下，`VTH≈3.06 V` 主要由网格造成的可能性有多大？请说明什么图像证据才足以判定是网格问题，不能只凭“网格可能影响结果”。

B. 如果不是网格主因，请按优先级排序：UID 净掺杂/补偿、Ni 有效功函数、固定界面电荷、界面态/体陷阱，并说明每一项对阈值、SS、On/Off 的预期方向。不要同时无约束调多项参数。

C. 在不改变几何、不做参数扫描的前提下，推荐下一次只改哪一个物理量或只增加哪一个诊断输出？若建议数值，必须区分“论文值”“继承值”“建议试探值”。

D. 是否需要额外保存 `VGS=3.1 V` 与 `VGS=9 V` 的 STR，并比较电子浓度/导带，才能区分提前积累与网格泄漏？

E. 请审查下面 RUN011 全代码中是否仍存在材料区号、电极归属、宽度归一化或求解路径错误；只指出具体行/语句和理由，不要重写整个 deck。

## RUN011 全代码

```text
# =============================================================================
# YUE2026_RUN011_DEPOSIT_IDVG_SMALL
#
# Purpose: first forward low-voltage electrical check of the RUN010 deposited
# topology.  This is not a BV or SEB run and does not tune physical parameters.
# Geometry/process values are frozen from the paper-note/RUN010 review.
# =============================================================================

go devedit

work.area x1=-0.30 y1=-1.05 x2=19.80 y2=5.23

# BetaGa2O3 epitaxy.
region id=5 name=source_nplus material="UD1(BetaGa2O3)" \
       points="0.0,0.00 3.0,0.00 3.0,0.03 0.0,0.03 0.0,0.00"
impurity id=1 region.id=5 imp=Donors peak.value=7.0e18 \
         ref.value=1.0e12 comb.func=Multiply

region id=6 name=drain_nplus material="UD1(BetaGa2O3)" \
       points="16.5,0.00 19.5,0.00 19.5,0.03 16.5,0.03 16.5,0.00"
impurity id=2 region.id=6 imp=Donors peak.value=7.0e18 \
         ref.value=1.0e12 comb.func=Multiply

region id=7 name=access_left material="UD1(BetaGa2O3)" \
       points="0.0,0.03 4.5,0.03 4.5,0.13 0.0,0.13 0.0,0.03"
impurity id=3 region.id=7 imp=Donors peak.value=1.0e18 \
         ref.value=1.0e12 comb.func=Multiply

region id=8 name=access_right material="UD1(BetaGa2O3)" \
       points="6.5,0.03 19.5,0.03 19.5,0.13 6.5,0.13 6.5,0.03"
impurity id=4 region.id=8 imp=Donors peak.value=1.0e18 \
         ref.value=1.0e12 comb.func=Multiply

region id=9 name=uid_channel material="UD1(BetaGa2O3)" \
       points="0.0,0.13 19.5,0.13 19.5,0.23 0.0,0.23 0.0,0.13"
impurity id=5 region.id=9 imp=Donors peak.value=5.0e15 \
         ref.value=1.0e12 comb.func=Multiply

region id=10 name=fe_substrate material="UD1(BetaGa2O3)" \
       points="0.0,0.23 19.5,0.23 19.5,5.23 0.0,5.23 0.0,0.23"
impurity id=6 region.id=10 imp=Donors peak.value=1.5e15 \
         ref.value=1.0e12 comb.func=Multiply

# Paper metal totals: S/D Ti/Au 60/180 nm -> 0.24 um Gold 2-D equivalent.
deposit material=Gold thickness=0.24 start=0.00 end=3.00 \
        region.id=81 region.name=source elec.id=1 automatic.join
deposit material=Gold thickness=0.24 start=16.50 end=19.50 \
        region.id=82 region.name=drain elec.id=2 automatic.join

# Paper Al2O3 thickness: 20 nm.  One continuous deposition.
deposit material=Al2O3 thickness=0.02 start=3.00 end=16.50 \
        region.id=83 region.name=gate_oxide automatic.join

# Paper gate Ni/Au 50/150 nm -> 0.20 um Nickel 2-D equivalent.
deposit material=Nickel thickness=0.20 start=4.00 end=7.00 \
        region.id=84 region.name=gate elec.id=3 automatic.join

# Paper PECVD SiO2 passivation: 300 nm.  This is not a metal thickness.
deposit material=SiO2 thickness=0.30 start=0.40 end=19.49 \
        region.id=85 region.name=passivation automatic.join

# Source via engineering window retained from validated RUN010.
region id=90 name=via_cut material=Silicon \
       points="0.40,-1.00 1.60,-1.00 1.60,-0.24 \
               0.40,-0.24 0.40,-1.00"
region delete id=90

# Source FP Ti/Au 60/180 nm -> 0.24 um Gold 2-D equivalent.
# Keep source_fp as a distinct region name; bind electrically with elec.id=1.
deposit material=Gold thickness=0.24 start=0.00 end=10.00 \
        region.id=86 region.name=source_fp elec.id=1

# Whole-region mesh only; no high-field-edge local refinement box.
base.mesh height=0.60 width=0.50
bound.cond !apply max.slope=30 max.ratio=50 rnd.unit=0.001 \
           line.straightening=0 align.points when=automatic
constr.mesh max.angle=90 max.ratio=50 \
            max.height=0.60 max.width=0.50 \
            min.height=0.001 min.width=0.001
constr.mesh reg=5 max.height=0.015 max.width=0.30
constr.mesh reg=6 max.height=0.015 max.width=0.30
constr.mesh reg=7 max.height=0.025 max.width=0.40
constr.mesh reg=8 max.height=0.025 max.width=0.40
constr.mesh reg=9 max.height=0.025 max.width=0.40
constr.mesh reg=10 max.height=0.50 max.width=0.60

mesh mode=MeshBuild
structure outfile="YUE2026_RUN011_DEPOSIT_IDVG_SMALL_mesh.str"

# =============================================================================
# ATLAS forward baseline: inherit the verified RUN005/RUN096 BetaGa2O3 model.
# user.default=GaN below is only ATLAS's numerical user-material template;
# all semiconductor regions remain explicitly named UD1(BetaGa2O3).
# =============================================================================

go atlas simflags="-V 5.40.0.R -P 4"
mesh infile="YUE2026_RUN011_DEPOSIT_IDVG_SMALL_mesh.str" width=20

material region=1 user.group=semiconductor user.default=GaN \
         affinity=4.00 eg300=4.85 egalph=0 egbeta=0 \
         permittivity=10.2 nc300=3.718e18 nv300=6.44e20 \
         taun0=1.2e-8 taup0=1.2e-8 \
         edb=0.06 gcb=2.0 eab=1.0 gvb=4.0 \
         augn=2.8e-31 augp=9.9e-32 \
         tcon.const tc.const=0.27 \
         hc.std hc.a=3.332 hc.b=0 hc.c=0 hc.d=0
material region=2 user.group=semiconductor user.default=GaN \
         affinity=4.00 eg300=4.85 egalph=0 egbeta=0 \
         permittivity=10.2 nc300=3.718e18 nv300=6.44e20 \
         taun0=1.2e-8 taup0=1.2e-8 \
         edb=0.06 gcb=2.0 eab=1.0 gvb=4.0 \
         augn=2.8e-31 augp=9.9e-32 \
         tcon.const tc.const=0.27 \
         hc.std hc.a=3.332 hc.b=0 hc.c=0 hc.d=0
material region=3 user.group=semiconductor user.default=GaN \
         affinity=4.00 eg300=4.85 egalph=0 egbeta=0 \
         permittivity=10.2 nc300=3.718e18 nv300=6.44e20 \
         taun0=1.2e-8 taup0=1.2e-8 \
         edb=0.06 gcb=2.0 eab=1.0 gvb=4.0 \
         augn=2.8e-31 augp=9.9e-32 \
         tcon.const tc.const=0.27 \
         hc.std hc.a=3.332 hc.b=0 hc.c=0 hc.d=0
material region=4 user.group=semiconductor user.default=GaN \
         affinity=4.00 eg300=4.85 egalph=0 egbeta=0 \
         permittivity=10.2 nc300=3.718e18 nv300=6.44e20 \
         taun0=1.2e-8 taup0=1.2e-8 \
         edb=0.06 gcb=2.0 eab=1.0 gvb=4.0 \
         augn=2.8e-31 augp=9.9e-32 \
         tcon.const tc.const=0.27 \
         hc.std hc.a=3.332 hc.b=0 hc.c=0 hc.d=0
material region=5 user.group=semiconductor user.default=GaN \
         affinity=4.00 eg300=4.85 egalph=0 egbeta=0 \
         permittivity=10.2 nc300=3.718e18 nv300=6.44e20 \
         taun0=1.2e-8 taup0=1.2e-8 \
         edb=0.06 gcb=2.0 eab=1.0 gvb=4.0 \
         augn=2.8e-31 augp=9.9e-32 \
         tcon.const tc.const=0.27 \
         hc.std hc.a=3.332 hc.b=0 hc.c=0 hc.d=0
material region=6 user.group=semiconductor user.default=GaN \
         affinity=4.00 eg300=4.85 egalph=0 egbeta=0 \
         permittivity=10.2 nc300=3.718e18 nv300=6.44e20 \
         taun0=1.2e-8 taup0=1.2e-8 \
         edb=0.06 gcb=2.0 eab=1.0 gvb=4.0 \
         augn=2.8e-31 augp=9.9e-32 \
         tcon.const tc.const=0.27 \
         hc.std hc.a=3.332 hc.b=0 hc.c=0 hc.d=0

material region=8 permittivity=9.0 tcon.const tc.const=0.33
material region=10 permittivity=3.9 tcon.const tc.const=0.014

mobility region=1 mu1n.caug=10 mu2n.caug=118.0 \
         ncritn.caug=2e17 deltan.caug=1.0 \
         alphan.caug=0 betan.caug=0 gamman.caug=0 \
         mu1p.caug=1e-6 mu2p.caug=1.3 \
         ncritp.caug=1e17 deltap.caug=1.0 \
         alphap.caug=0 betap.caug=0 gammap.caug=0 \
         vsatn=2.0e7 vsatp=2.0e7 betan=7.52 betap=7.52
mobility region=2 mu1n.caug=10 mu2n.caug=118.0 \
         ncritn.caug=2e17 deltan.caug=1.0 \
         alphan.caug=0 betan.caug=0 gamman.caug=0 \
         mu1p.caug=1e-6 mu2p.caug=1.3 \
         ncritp.caug=1e17 deltap.caug=1.0 \
         alphap.caug=0 betap.caug=0 gammap.caug=0 \
         vsatn=2.0e7 vsatp=2.0e7 betan=7.52 betap=7.52
mobility region=3 mu1n.caug=10 mu2n.caug=118.0 \
         ncritn.caug=2e17 deltan.caug=1.0 \
         alphan.caug=0 betan.caug=0 gamman.caug=0 \
         mu1p.caug=1e-6 mu2p.caug=1.3 \
         ncritp.caug=1e17 deltap.caug=1.0 \
         alphap.caug=0 betap.caug=0 gammap.caug=0 \
         vsatn=2.0e7 vsatp=2.0e7 betan=7.52 betap=7.52
mobility region=4 mu1n.caug=10 mu2n.caug=118.0 \
         ncritn.caug=2e17 deltan.caug=1.0 \
         alphan.caug=0 betan.caug=0 gamman.caug=0 \
         mu1p.caug=1e-6 mu2p.caug=1.3 \
         ncritp.caug=1e17 deltap.caug=1.0 \
         alphap.caug=0 betap.caug=0 gammap.caug=0 \
         vsatn=2.0e7 vsatp=2.0e7 betan=7.52 betap=7.52
mobility region=5 mu1n.caug=10 mu2n.caug=118.0 \
         ncritn.caug=2e17 deltan.caug=1.0 \
         alphan.caug=0 betan.caug=0 gamman.caug=0 \
         mu1p.caug=1e-6 mu2p.caug=1.3 \
         ncritp.caug=1e17 deltap.caug=1.0 \
         alphap.caug=0 betap.caug=0 gammap.caug=0 \
         vsatn=2.0e7 vsatp=2.0e7 betan=7.52 betap=7.52
mobility region=6 mu1n.caug=10 mu2n.caug=118.0 \
         ncritn.caug=2e17 deltan.caug=1.0 \
         alphan.caug=0 betan.caug=0 gamman.caug=0 \
         mu1p.caug=1e-6 mu2p.caug=1.3 \
         ncritp.caug=1e17 deltap.caug=1.0 \
         alphap.caug=0 betap.caug=0 gammap.caug=0 \
         vsatn=2.0e7 vsatp=2.0e7 betan=7.52 betap=7.52

# RUN096 Fe-related substrate trap; unchanged.
trap region=6 acceptor e.level=0.8 density=2.0e18 \
     degen.fac=1 sign=5.0e-15 sigp=5.0e-15

# DevEdit retains source_fp as the electrode-1 name after the final deposit.
# It is the physical Source terminal because both use elec.id=1.
contact name=source_fp workfunc=4.00
contact name=drain workfunc=4.00
contact name=gate workfunc=5.78

models analytic fldmob srh auger fermi incomplete print
method newton trap maxtraps=10 climit=1e-4 itlimit=50
output e.field flowlines e.mobility h.mobility band.param \
       ex.field ey.field jx.electron jy.electron \
       jx.hole jy.hole jx.total jy.total

solve init
save outfile="YUE2026_RUN011_DEPOSIT_IDVG_SMALL_init.str"

solve vgate=0 name=gate
solve vdrain=0.01 name=drain
solve vdrain=0.10 name=drain previous
solve vdrain=0.50 name=drain previous
solve vdrain=1.00 name=drain previous
save outfile="YUE2026_RUN011_DEPOSIT_IDVG_SMALL_vd1_vg0.str"

# Paper small-periphery transfer condition: Wch=20 um and VDS=1 V.
log outfile="YUE2026_RUN011_DEPOSIT_IDVG_SMALL.log"
solve vgate=0 name=gate
solve vstep=0.10 vfinal=12.0 name=gate
save outfile="YUE2026_RUN011_DEPOSIT_IDVG_SMALL_vd1_vg12.str"
log off

quit
```

