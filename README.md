# Porious-Media-Transfer# 🧱 Porous-Media Transfer — From Pore Scale to 3-D Aquifer  
*Micro-TDM – ENSEEIHT 2ⁿᵈ year*

How do you go from a bunch of cylinders to a working reservoir model ?  
We answer step-by-step with **COMSOL**, **coffee-break maths** and a lot of pictures.

---

## I – Pore scale : “Stokes in a maze”  
*2 m × 2 m slices packed with cylinders (ϕ = 0.72 – 0.86)*

We generate three periodic arrays differing only in cylinder diameter and count, then solve **Stokes** for 0.001 ≤ Re ≤ 400.
<img width="790" height="442" alt="Capture d’écran 2025-11-21 à 16 51 14" src="https://github.com/user-attachments/assets/4581da04-7dc5-4cd0-888a-1c9c93ce2bb1" />

### I-A  Theory reminder  
Porosity and specific surface:

$$
\varepsilon = \frac{S - S_{\text{solid}}}{S}, \quad S_{\text{spec}} = \frac{4}{D}
$$

Kozeny & Kozeny–Carman give **analytical** permeabilities:

$$
k_{\text{Koz}} = C_{0}\frac{\varepsilon^{3}}{S_{\text{spec}}^{2}}, \quad
k_{\text{K-C}} = \frac{d_{\text{m}}^{2}}{180}\frac{\varepsilon^{3}}{(1-\varepsilon)^{2}}
$$

### I-B  Numerical campaign  
Record ΔP(V) and fit **Darcy–Forchheimer**:

$$
-\nabla P = \underbrace{\frac{\mu}{k}\,V}_{\text{Darcy}} + \underbrace{\beta\rho\,V^{2}}_{\text{Forchheimer}}
$$

| Medium | k Kozeny (m²) | k DNS-low-Re (m²) | β (m⁻¹) |
|--------|---------------|-------------------|---------|
| 1      | 9.2×10⁻³      | 8.4×10⁻⁴          | 1.1     |
| 2      | 7.9×10⁻³      | 3.2×10⁻³          | 0.8     |
| 3      | 7.0×10⁻³      | 2.2×10⁻³          | 0.9     |

**Take-away**: Kozeny **over-predicts k by ≈ 10×** – calibrate on micro-CT !

<img width="816" height="386" alt="Capture d’écran 2025-11-21 à 16 51 52" src="https://github.com/user-attachments/assets/44d637c3-95bb-42c8-95fe-f10ad2f3697b" />


<img width="859" height="501" alt="Capture d’écran 2025-11-21 à 16 52 18" src="https://github.com/user-attachments/assets/08222ebb-5eb7-43bf-9f1f-21fb47779e3c" />


---

## II – Core scale : “Layer-cake aquifer”  
*10 m thick stratified slab – two flow directions, two stories*

We build a 3-layer sandwich (high-k / low-k / high-k) and impose the **same** 50 kPa drop in two orientations.

### II-A  Horizontal flow – “layers in parallel”  
Derivation gives:

$$
k_{\text{h}} = \frac{\sum k_{j}L_{j}}{L}
$$

COMSOL velocity map shows **jump discontinuities** at interfaces, yet the **average flux** matches the homogeneous equivalent within **0.3 %** – the formula works.

![Stratified-horizontal](figures/strat_h.png)

### II-B  Vertical flow – “layers in series”  
Derivation gives:

$$
k_{\text{v}} = \frac{L}{\sum L_{j}/k_{j}}
$$

Now the **pressure field piles up** inside the tight layer and the global flow rate is again within **1 %** of theory – but the **local gradient is nowhere constant**.

![Stratified-vertical](figures/strat_v.png)

**Key visual**: same colour-bar, same ΔP, **completely different** pressure & velocity landscapes – yet the upscaled number is exact as long as the macroscopic gradient stays aligned with the layers.

---

## III – Field scale : “3-D injection”  
*100 m block with surface tank and bottom well*

We move to a **real-field geometry**: 100 m × 100 m × 100 m block with a 10 m × 10 m injection box on top and a bottom outlet.  
Boundary condition: **constant influx** 0.0001 m s⁻¹.

### III-A  Set-up  
- Use **kᵥ** from TD2 as the *homogeneous* permeability  
- Run both **stratified** and **equivalent-homogeneous** models  
- Extract **ΔP** between injection and extraction planes

### III-B  Results  
| Quantity | Stratified | Homogeneous | Δ |
|----------|------------|-------------|---|
| ΔP (Pa)  | 1.37×10⁻⁴  | 1.09×10⁻⁴   | **+26 %** |
| Sweep volume | larger | smaller | — |

**Conclusion**: as soon as streamlines **bend**, the single *kᵥ* is **not enough** – full tensor or explicit layers are required.

![3-D streamlines](figures/3D_streamlines.png)  
*Red = streamlines, grey = iso-pressure – bending through high-k layers*

![Iso-pressure slice](figures/3D_p_iso.png)  
*Discontinuous pressure planes at layer joints*

---

## 🧰 Repository Structure
