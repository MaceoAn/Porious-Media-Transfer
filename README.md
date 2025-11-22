# 🧱 Porous-Media Transfer — From Pore Scale to 3-D Aquifer  
*Micro-TDM – ENSEEIHT (2ᵉ année)*

This project explores fluid transport in porous media from the **microscopic pore geometry** to the **macroscopic aquifer scale**, combining:

- COMSOL Multiphysics simulations  
- Classical porous media theory (Kozeny, Kozeny–Carman, Darcy–Forchheimer)  
- Upscaling methods  
- A final 3-D field-scale injection model  

The aim is to understand how **porosity, permeability and flow patterns** evolve across scales, and when **equivalent homogeneous models** remain valid.

---

# I — Pore Scale: Stokes Flow Through Cylinder Arrays  
Three 2 m × 2 m periodic media were generated, each containing a square lattice of cylinders with different diameters → different porosities and specific surfaces.

COMSOL solves **Stokes flow** across 0.001 ≤ Re ≤ 400 to extract pressure drop and infer permeability.

Picture of our Three configuration 
<img width="855" height="442" alt="Capture d’écran 2025-11-22 à 13 49 48" src="https://github.com/user-attachments/assets/29b99a4f-9d5d-47bf-9365-52c06ae3cc62" />

---

## I-A — Theory

### **Porosity**
$$
\varepsilon = \frac{S - S_{\text{solid}}}{S}
$$

### **Specific surface**
$$
S_{\text{spec}} = \frac{4}{D}
$$

### **Kozeny and Kozeny–Carman permeabilities**
$$
k_{\text{Koz}} = C_0 \frac{\varepsilon^3}{S_{\text{spec}}^2}
$$

$$
k_{\text{K-C}} = \frac{d_m^2}{180}\frac{\varepsilon^3}{(1 - \varepsilon)^2}, 
\qquad 
d_m = \frac{6(1 - \varepsilon)}{S_{\text{spec}}}
$$

These give *analytical* permeability estimates for comparison with numerical results.


---

## I-B — Numerical Procedure

We impose a velocity and measure the pressure drop ΔP to reconstruct  
the **Darcy–Forchheimer law**:

$$
-\nabla P = \frac{\mu}{k}\,V + \beta \rho V^2
$$

- Low-Re region → linear fit gives \(k\)  
- High-Re → non-linearity determines \(\beta\)

| Medium | k Kozeny (m²) | k DNS low-Re (m²) | β (m⁻¹) |
|--------|----------------|-------------------|----------|
| 1 | 9.2×10⁻³ | 8.4×10⁻⁴ | 1.1 |
| 2 | 7.9×10⁻³ | 3.2×10⁻³ | 0.8 |
| 3 | 7.0×10⁻³ | 2.2×10⁻³ | 0.9 |
## 🔍 Fig. 2 – Finite-Element Mesh of the Porous Medium

The computational mesh is strongly refined near the cylinder walls to resolve
boundary layers, while the central region remains moderately refined.

<p align="center">
  <img width="837" height="421" alt="Capture d’écran 2025-11-22 à 13 52 57" src="https://github.com/user-attachments/assets/0fd6e84a-cccd-4709-9bde-2257e83a029f" />
</p>

— Unstructured triangular mesh used in the pore-scale simulations.*

## – Velocity & Pressure Fields (Low vs High Reynolds)

We visualize the Stokes flow for two regimes:
- **Low Reynolds** (creeping flow)
- **High Reynolds** (inertial effects visible)

<p align="center">
  <img width="837" height="517" alt="Capture d’écran 2025-11-22 à 13 53 08" src="https://github.com/user-attachments/assets/754fe6ce-105a-435d-911a-5e507970b58b" />
</p>

*Figure 3 — (a) Velocity field at low Re, (b) velocity field at high Re,  
(c) pressure field at low Re, (d) pressure field at high Re.*

These visualizations illustrate the transition from a purely viscous regime to
a regime where inertial effects distort streamlines and amplify pressure gradients.
**Key conclusion:**  
Kozeny **systematically overestimates permeability by ~×10**.  
DNS-based calibration is necessary for realistic media.

---

# II — Core Scale: Stratified 2-D Medium  
We model a 10 m thick slab with **three horizontal layers**, using permeabilities from Part I.
<img width="541" height="334" alt="Capture d’écran 2025-11-22 à 13 51 56" src="https://github.com/user-attachments/assets/d511a6f9-e437-4601-89a7-e3db8f633631" />
Two configurations are tested:

- **Horizontal flow** (parallel to layers)  
- **Vertical flow** (across layers)

---

## II-A — Horizontal Flow (layers in parallel)

Equivalent permeability:

$$
k_h = \frac{1}{L}\sum_j k_j L_j
$$

COMSOL:

- Velocity jumps at interfaces  
- Average flux matches theory within **0.3 %**


👉 **Parallel formula perfectly valid.**

---

## II-B — Vertical Flow (layers in series)

Equivalent permeability:

$$
k_v = \frac{L}{\sum_j L_j / k_j}
$$

COMSOL:

- Pressure accumulates inside the low-k layer  
- Total flow rate matches homogenised medium within **1 %**

👉 Homogenisation is valid for **bulk flux**, but the **internal pressure field is not captured**.

---

# III — Field Scale: 3-D Injection Through Stratified Aquifer  
We upscale to a **100 m × 100 m × 100 m** domain with:

- A 10 m × 10 m injection box on top  
- A bottom extraction plane  
- Uniform inflow \(U_0 = 10^{-4}\,\text{m·s}^{-1}\)

Two models:

1. **Explicit 3-D stratified permeability**  
2. **Homogeneous equivalent medium** using \(k_v\)

---

## III-A — Results

| Quantity | Stratified | Homogeneous | Δ |
|----------|------------|-------------|-----|
| ΔP (Pa)  | 1.37×10⁻⁴ | 1.09×10⁻⁴ | +26 % |

Observations:

- Streamlines curve as they cross layers  
- Isopressure surfaces become discontinuous  
- Homogenised model underestimates ΔP significantly

👉 Once flow becomes **3-D**, equivalent permeability **breaks down**.  
A full **permeability tensor** or explicit layers are required.

---

# 🧩 Final Conclusions

Across scales, we observe:

- Microscale geometry dictates permeability  
- Darcy’s law holds only at low Re → Forchheimer needed after  
- Upscaling works in 2-D **only if flow is aligned with layering**  
- In 3-D, streamlines bend → homogenisation fails (~25% ΔP error)  
- Internal pressure and velocity structures require **heterogeneous models**

This TD demonstrates the **limits of classical homogenisation** and shows why multi-scale modelling is essential in hydrogeology and porous-media engineering.

---

# 📂 Repository Structure (suggested)
