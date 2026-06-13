# Finite Element Method (FEM) Reference Guide
## Canonical B-Matrices, D-Matrices, Shape Functions, and Stiffness Formulations
This document serves as a comprehensive reference for the finite element formulations of standard 1D, 2D, and 3D elements. It covers the fundamental shape functions, strain-displacement matrices (B), constitutive matrices (D), and elemental stiffness matrices (K_e).
## 1. General Linear Elastic FEM Formulation
For any finite element, the stiffness matrix is derived by integrating over the element domain \Omega_e:
Using numerical integration (e.g., Gauss quadrature), this becomes:
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
## 2. 1D Elements
### 1D Bar Element
 * **Nodes:** 2
 * **Degrees of Freedom (DOF):** [u_1, u_2]
 * **Matrix Sizes:** B is 1 \times 2, D is 1 \times 1
**Shape Functions:**

**Displacement Field:**

**Matrices:**

### Euler-Bernoulli Beam
 * **Nodes:** 2
 * **Degrees of Freedom (DOF):** [v_1, \theta_1, v_2, \theta_2]
**Cubic Hermite Shape Functions:**

**Matrices:**

## 3. 2D Elements
### Constant Strain Triangle (CST)
 * **Nodes:** 3
 * **B-Matrix Size:** 3 \times 6
**Shape Functions (i = 1, 2, 3):**

**B-Matrix:**

**D-Matrix (Plane Stress):**

**D-Matrix (Plane Strain):**

### Linear Strain Triangle (LST / 6-Node)
 * **Nodes:** 6
 * **Coordinate System:** Area Coordinates (L_1+L_2+L_3=1)
 * **B-Matrix Size:** 3 \times 12
**Shape Functions:**
 * Corner Nodes: N_1=L_1(2L_1-1), N_2=L_2(2L_2-1), N_3=L_3(2L_3-1)
 * Mid-side Nodes: N_4=4L_1L_2, N_5=4L_2L_3, N_6=4L_3L_1
**B-Matrix Structure:**

### Q4 Quadrilateral
 * **Nodes:** 4
 * **Coordinate System:** Natural Coordinates (\xi, \eta)
 * **B-Matrix Size:** 3 \times 8
**Shape Functions:**

**B-Matrix Structure:**

### Q8 Serendipity Element
 * **Nodes:** 8
 * **B-Matrix Size:** 3 \times 16
**Shape Functions:**
 * Corner Node: 
 * Mid-side Node: 
**B-Matrix Structure:**

## 4. 3D Elements
### TET4 (4-Node Tetrahedron)
 * **Nodes:** 4
 * **B-Matrix Size:** 6 \times 12
**Shape Functions:**

**B-Matrix:**

### TET10 (10-Node Tetrahedron)
 * **Nodes:** 10
 * **B-Matrix Size:** 6 \times 30
**Quadratic Shape Functions:**
 * Corner Nodes: N_i=L_i(2L_i-1)
 * Edge Nodes: N_{ij}=4L_iL_j
**B-Matrix Structure:**

### HEX8 / SOLID185 (8-Node Hexahedron)
 * **Nodes:** 8
 * **B-Matrix Size:** 6 \times 24
**Shape Functions:**

**B-Matrix Structure:**

### HEX20 / SOLID186 (20-Node Hexahedron)
 * **Nodes:** 20
 * **B-Matrix Size:** 6 \times 60
**Quadratic Shape Functions:**
 * Corner Node: 
 * Edge Node (Example): 
**B-Matrix Structure:**

### HEX27 (Complete Quadratic Brick)
 * **Nodes:** 27
 * **B-Matrix Size:** 6 \times 81
**1D Quadratic Basis Functions:**

**Shape Functions (Total: 27):**

**B-Matrix Structure:**

## 5. General Anisotropic Material Matrix
For the most general linear elastic constitutive law (\sigma = D\varepsilon), the D matrix represents full anisotropy.
 * **Independent Constants:** 21
## Key Conclusions
For every formulation in the Finite Element Method:
 1. **Shape Functions (N)** define the interpolation within the element.
 2. **Derivatives of Shape Functions** define the strain-displacement matrix (B).
 3. **The Material Law** defines the constitutive matrix (D).
Changing the material assumption (e.g., from isotropic to orthotropic, transversely isotropic, or fully anisotropic) **only** changes the D matrix. The element's geometric interpolation (N) and its kinematic mapping (B-matrix formulation) remain completely unaffected.
