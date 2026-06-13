# Finite Element Method (FEM) Reference Guide
## Canonical B-Matrices, D-Matrices, Shape Functions, and Stiffness Formulations
This document serves as a comprehensive reference for the finite element formulations of standard 1D, 2D, and 3D elements. It covers fundamental shape functions, strain-displacement matrices (B), constitutive matrices (D), and elemental stiffness matrices (K_e), including material definitions and performance characteristics.
## 1. General Linear Elastic FEM Formulation
For any finite element, the stiffness matrix is derived by integrating over the element domain \Omega_e:
Using numerical integration (e.g., Gauss quadrature), this becomes:
### Single Element Direct Substitution Form
For elements with a constant B-matrix (constant strain elements like the 1D Bar, 2D CST, or 3D TET4), the integration simplifies directly to a multiplication of matrices and the element volume/area:
*(Where V_e is the volume, area \times thickness, or length \times cross-section depending on the dimension).*
**Variable Definitions:**
| Symbol | Meaning |
|---|---|
| K_e | Element stiffness matrix |
| B | Strain-displacement matrix |
| D | Constitutive (material) matrix |
| J | Jacobian matrix |
| w | Gauss weight |
| \Omega | Element domain |
### Fundamental Relationships
The entire linear elastic FEM framework relies on these three foundational equations:
 * **Strain-Displacement:** \varepsilon = B u
 * **Stress-Strain:** \sigma = D \varepsilon
 * **Stiffness:** K_e = \int B^T D B\,d\Omega
## 2. Material Constitutive Models (D-Matrices)
The constitutive matrix D links stress to strain. Changing the material type alters **only** the D matrix, leaving the shape functions (N) and B-matrices completely unaffected.
### Isotropic Material
Material properties are identical in all directions. It requires only **2 independent constants**: Young's Modulus (E) and Poisson's ratio (\nu).
For 3D solids:

### Orthotropic Material
Material properties differ along three mutually orthogonal axes (e.g., wood, rolled steel, unidirectional composites). It requires **9 independent constants** (E_x, E_y, E_z, \nu_{xy}, \nu_{yz}, \nu_{xz}, G_{xy}, G_{yz}, G_{xz}).
The easiest way to define this is via the inverse compliance relationship (D^{-1}):

*(Note: Symmetry requires \frac{\nu_{ij}}{E_i} = \frac{\nu_{ji}}{E_j})*
### Fully Anisotropic Material
The most general linear elastic material with no planes of symmetry.
 * **Independent Constants:** 21
## 3. 1D Elements
### 1D Bar Element (Truss)
 * **Nodes:** 2
 * **Degrees of Freedom:** [u_1, u_2]
 * **Performance:** Exact for constant strain/linear displacement fields. Extremely fast compute speeds.
**Shape Functions:**

**Displacement Field:**

**Matrices:**

### Euler-Bernoulli Beam
 * **Nodes:** 2
 * **Degrees of Freedom:** [v_1, \theta_1, v_2, \theta_2]
 * **Performance:** Exact for cubic transverse displacements (shear deformation neglected). Highly efficient for slender beams.
**Cubic Hermite Shape Functions:** (\xi = \frac{x}{L})

**Matrices:**

## 4. 2D Elements
### Constant Strain Triangle (CST / 3-Node)
 * **Nodes:** 3
 * **Size:** B is **3x6**
 * **Performance:** Fast computation, but **poor accuracy** in bending regions (suffers from severe shear locking). Slow convergence (\mathcal{O}(h) for strains). Use only for fine meshes in bulk areas.
**Shape Functions:** (i = 1, 2, 3)


*(where b_1 = y_2 - y_3, c_1 = x_3 - x_2, etc., in cyclic order)*
**B-Matrix:**

**D-Matrix (Plane Stress):**

**D-Matrix (Plane Strain):**

### Linear Strain Triangle (LST / 6-Node)
 * **Nodes:** 6
 * **Coordinate System:** Area Coordinates (L_1+L_2+L_3=1)
 * **Size:** B is **3x12**
 * **Performance:** High accuracy. Prevents shear locking and captures high-strain gradients well. Quadratic convergence (\mathcal{O}(h^2) for strains).
**Shape Functions:**
 * Corner Nodes: N_1=L_1(2L_1-1), \quad N_2=L_2(2L_2-1), \quad N_3=L_3(2L_3-1)
 * Mid-side Nodes: N_4=4L_1L_2, \quad N_5=4L_2L_3, \quad N_6=4L_3L_1
**B-Matrix Structure:**

### Q4 Quadrilateral
 * **Nodes:** 4
 * **Coordinate System:** Natural Coordinates (\xi, \eta)
 * **Size:** B is **3x8**
 * **Performance:** Better balance of efficiency than CST, but susceptible to parasitic shear locking in bending fields unless selective reduced integration is implemented.
**Shape Functions:**

**B-Matrix Structure:**

### Q8 Serendipity Element
 * **Nodes:** 8
 * **Size:** B is **3x16**
 * **Performance:** Highly accurate. The standard robust choice for 2D structured mapping. Resists edge distortion well with high convergence parameters.
**Shape Functions:**
 * Corner Nodes: N_i = \frac{1}{4}(1+\xi_i\xi)(1+\eta_i\eta)(\xi_i\xi+\eta_i\eta-1)
 * Mid-side Nodes (\xi_i=0): N_i = \frac{1}{2}(1-\xi^2)(1+\eta_i\eta)
 * Mid-side Nodes (\eta_i=0): N_i = \frac{1}{2}(1-\eta^2)(1+\xi_i\xi)
**B-Matrix Structure:**

## 5. 3D Elements
### TET4 (4-Node Tetrahedron)
 * **Nodes:** 4
 * **Size:** B is **6x12**
 * **Performance:** Fastest 3D element to calculate but **extremely stiff**. Suffers from severe volumetric locking and shear locking. **Avoid for accurate stress analysis** unless using exceptionally fine meshes.
**Shape Functions:**

**B-Matrix:**

### TET10 (10-Node Tetrahedron) — ⭐ Best Choice for Complex Geometries
 * **Nodes:** 10
 * **Size:** B is **6x30**
 * **Performance:** The industry workhorse for complex 3D CAD geometries. Quadratic interpolation bypasses locking, providing excellent convergence properties and handling irregular meshes seamlessly.
**Quadratic Shape Functions:**
 * Corner Nodes: N_i = L_i(2L_i-1) \quad \text{for } i=1,2,3,4
 * Edge Nodes: N_{ij} = 4L_iL_j
**B-Matrix Structure:**

### HEX8 / SOLID185 (8-Node Hexahedron)
 * **Nodes:** 8
 * **Size:** B is **6x24**
 * **Performance:** Computationally efficient for regular structured meshes. Still prone to shear locking under bending unless saved by enhanced strain formulations or selective reduced integration schemes.
**Shape Functions:**

**B-Matrix Structure:**

### HEX20 / SOLID186 (20-Node Hexahedron) — ⭐ Best Overall Accuracy
 * **Nodes:** 20
 * **Size:** B is **6x60**
 * **Performance:** Supreme mathematical accuracy for structural solid mechanics. Highest convergence rates and immune to locking issues. Hex-meshing irregular structures can be difficult, but when achieved, this element delivers the best accuracy-to-cost ratio.
**Quadratic Shape Functions:**
 * Corner Nodes: N_i = \frac{1}{8}(1+\xi_i\xi)(1+\eta_i\eta)(1+\zeta_i\zeta)(\xi_i\xi+\eta_i\eta+\zeta_i\zeta-2)
 * Edge Nodes (Example for \xi_i=0): N_i = \frac{1}{4}(1-\xi^2)(1+\eta_i\eta)(1+\zeta_i\zeta)
**B-Matrix Structure:**

### HEX27 (Complete Quadratic Brick)
 * **Nodes:** 27 (Includes face and volume internal center nodes)
 * **Size:** B is **6x81**
 * **Performance:** Maximum mathematical completeness for a quadratic basis. High computational overhead limits its general commercial use compared to HEX20, but it remains heavily utilized in high-order spectral methods.
**1D Quadratic Basis Functions:**

**Shape Functions (Total: 27):**

**B-Matrix Structure:**

