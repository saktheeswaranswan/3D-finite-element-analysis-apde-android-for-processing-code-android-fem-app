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


For a general multiphysics FEM solver, nearly every element matrix can be classified into a few canonical forms.

Master FEM Equation

K_e=\int_{\Omega_e} B^T D B\,d\Omega

K_e=\int_{\Omega_e} B^T D B\, d\Omega

Many physics problems add mass, convection, damping, coupling, and source matrices.


---

1. Solid Mechanics (Structural FEM)

Unknowns:

u,v,w

Element stiffness:

K_s=\int B^T D B\,dV

Constitutive matrix:

3D Isotropic Elasticity

D=
\frac{E}{(1+\nu)(1-2\nu)}
\begin{bmatrix}
1-\nu&\nu&\nu&0&0&0\\
\nu&1-\nu&\nu&0&0&0\\
\nu&\nu&1-\nu&0&0&0\\
0&0&0&\frac{1-2\nu}{2}&0&0\\
0&0&0&0&\frac{1-2\nu}{2}&0\\
0&0&0&0&0&\frac{1-2\nu}{2}
\end{bmatrix}

Mass matrix:

M=\int \rho N^T N\,dV

Damping:

C=\alpha M+\beta K

Dynamic equation:

M\ddot u+C\dot u+Ku=F


---

2. Thermal FEM

Unknown:

T

Conductivity matrix:

K_t=
\int
(\nabla N)^T
k
(\nabla N)
\,dV

Capacity matrix:

C_t=
\int
\rho c_p N^TN
\,dV

Convection boundary matrix:

H=
\int_{\Gamma}
hN^TN
\,d\Gamma

Heat source vector:

F_q=
\int_V
N^TQ
\,dV

Transient system:

C_t\dot T+K_tT=F


---

3. CFD (Navier-Stokes)

Unknowns:

u,v,w,p


---

Mass Matrix

M=
\int
\rho N^TN
\,dV


---

Viscous Diffusion Matrix

K_v=
\int
B^T\mu B
\,dV


---

Convection Matrix

K_c=
\int
N^T\rho
(u\cdot\nabla N)
\,dV


---

Pressure Gradient Matrix

G=
\int
B_p^TN_u
\,dV


---

Divergence Matrix

D=
\int
N_p^TB_u
\,dV


---

SUPG Stabilization

K_{SUPG}
=
\int
\tau
(u\cdot\nabla N)^T
(u\cdot\nabla N)
\,dV


---

4. Electromagnetics FEM

Unknown:

A

(Magnetic vector potential)


---

Magnetostatic Matrix

From Maxwell:

\nabla\times
\left(
\frac1\mu
\nabla\times A
\right)
=
J

Element matrix:

K_m=
\int
(\nabla\times N)^T
\frac1\mu
(\nabla\times N)
\,dV

Source vector:

F=
\int
N^TJ
\,dV


---

Eddy Current Matrix

Conductivity matrix:

C_e=
\int
\sigma N^TN
\,dV

System:

C_e\dot A+K_mA=F


---

5. Electrostatics FEM

Unknown:

V

Potential equation:

\nabla\cdot
(\epsilon\nabla V)
=
-\rho

Stiffness matrix:

K_e=
\int
(\nabla N)^T
\epsilon
(\nabla N)
\,dV

Charge vector:

F=
\int
N^T\rho
\,dV

Observe:

Electrostatics stiffness is mathematically identical to thermal conductivity.


---

6. Piezoelectric FEM

Coupled displacement + voltage

Unknowns:

\{u,\phi\}

Mechanical stiffness:

K_{uu}
=
\int
B^TC^EB
\,dV

Electromechanical coupling:

K_{u\phi}
=
\int
B^Te^TB_\phi
\,dV

Electrical stiffness:

K_{\phi\phi}
=
\int
B_\phi^T
\epsilon
B_\phi
\,dV

Coupled system:

\begin{bmatrix}
K_{uu} & K_{u\phi}\\
K_{\phi u} & K_{\phi\phi}
\end{bmatrix}


---

7. Thermoelastic FEM

Coupling matrix:

F_T
=
\int
B^T
D
\alpha
\Delta T
\,dV

System:

Ku=F+F_T


---

8. Acoustic FEM

Pressure unknown:

p

Stiffness matrix:

K_a=
\int
(\nabla N)^T
\frac1\rho
(\nabla N)
\,dV

Mass matrix:

M_a=
\int
\frac1{c^2}
N^TN
\,dV

Wave equation:

M_a\ddot p+K_ap=F


---

9. Diffusion / Species Transport

Species concentration:

C

Diffusion matrix:

K_d=
\int
(\nabla N)^T
D
(\nabla N)
\,dV

Convection matrix:

K_c=
\int
N^T
(u\cdot\nabla N)
\,dV

Mass matrix:

M=
\int
N^TN
\,dV

System:

M\dot C+(K_d+K_c)C=F


---

Most Important Universal FEM Matrices

Matrix	Formula	Physics

Stiffness		Structural
Conductivity		Thermal
Diffusion		CFD/Species
Magnetic Curl-Curl		EM
Mass		Dynamic
Capacity		Thermal
Convection		CFD
Damping		Structural
Pressure Coupling	 matrices	CFD
Piezo Coupling		Multiphysics


These matrices form the core of most commercial FEM solvers such as ANSYS Mechanical, COMSOL Multiphysics, Abaqus, and OpenFOAM when solving structural, thermal, CFD, electromagnetic, acoustic, and coupled multiphysics problems.
https://github.com/NGSolve/ngsolve.git

https://github.com/mfem/mfem.git

https://github.com/oofem/oofem.git

https://github.com/crobarcro/xfemm.git

https://github.com/ElmerCSC/elmerfem.git

If you want actual GitHub source code that assembles the numerical element matrices (, , , mass matrices, thermal conductivity matrices, Navier-Stokes matrices, Maxwell curl-curl matrices), these are among the best open-source references:

Multiphysics (Structural + Thermal + CFD + Electromagnetics)

[NGSolve GitHub](https://github.com/NGSolve/ngsolve?utm_source=chatgpt.com)

Solid mechanics

Heat transfer

CFD

Electromagnetics

Source contains element integrators directly implementing



\int B^TDB\,dV

[MFEM GitHub](https://github.com/mfem/mfem?utm_source=chatgpt.com)

High-order FEM

Structural

Thermal

CFD

Electromagnetics

Excellent examples showing element matrix assembly and quadrature loops. 




---

Structural FEM (Best for Learning Stiffness Matrix Assembly)

[OOFEM GitHub](https://github.com/oofem/oofem?utm_source=chatgpt.com)

HEX8

HEX20

TET4

TET10

Beam

Shell

Plate elements

Explicit -matrix and stiffness assembly code. 




---

Electromagnetics FEM

[xfemm GitHub](https://github.com/REOptimize-Systems/xfemm?utm_source=chatgpt.com)

Magnetostatics

Eddy currents

Maxwell FEM

Curl-curl element matrices

Sparse assembly implementation. 




---

Complete Multiphysics Solver

[Elmer FEM GitHub](https://github.com/ElmerCSC/elmerfem?utm_source=chatgpt.com)

Structural

Thermal

CFD

Electromagnetics

Acoustics

Coupled multiphysics

Contains thousands of element formulations. 




---

Where the Actual Numerical Matrices Are

For a HEX20 element, look for source files implementing:

for(gp=0; gp<27; gp++)
{
    computeShapeFunctions();

    computeDerivatives();

    J = dN_dXi * coords;

    invJ = inverse(J);

    B = buildB(invJ,dN_dXi);

    Ke += transpose(B) * D * B * detJ * weight;
}

This loop appears in nearly every structural, thermal, and CFD FEM code.

The only thing that changes is the constitutive matrix:

Structural

K=
\int B^T D B\,dV

Thermal

K=
\int (\nabla N)^T k (\nabla N)\,dV

CFD Diffusion

K=
\int (\nabla N)^T \mu (\nabla N)\,dV

Electrostatics

K=
\int (\nabla N)^T \epsilon (\nabla N)\,dV

Magnetostatics

K=
\int (\nabla\times N)^T
\mu^{-1}
(\nabla\times N)\,dV

K=\int (\nabla\times N)^T\mu^{-1}(\nabla\times N)\,dV


---

For learning every important FEM element matrix from source code, I would study them in this order:

1. [MFEM GitHub](https://github.com/mfem/mfem?utm_source=chatgpt.com) (cleanest numerical formulations)


2. [NGSolve GitHub](https://github.com/NGSolve/ngsolve?utm_source=chatgpt.com) (multiphysics formulations)


3. [Elmer FEM GitHub](https://github.com/ElmerCSC/elmerfem?utm_source=chatgpt.com) (industrial-scale multiphysics)


4. [OOFEM GitHub](https://github.com/oofem/oofem?utm_source=chatgpt.com) (excellent structural element implementations)


5. [xfemm GitHub](https://github.com/REOptimize-Systems/xfemm?utm_source=chatgpt.com) (electromagnetic FEM)

https://fenicsproject.org/?utm_source=chatgpt.com



If you want Python-based FEM/CFD codes where the weak forms are written almost exactly as the mathematical equations, these are the most important projects:

1. [FEniCS Project](https://fenicsproject.org/?utm_source=chatgpt.com)

GitHub:

[FEniCS GitHub Organization](https://github.com/FEniCS?utm_source=chatgpt.com)

The key idea is that you write the weak form directly.

Structural Elasticity

a = inner(sigma(u), epsilon(v))*dx

which corresponds to

K=\int B^TDB\,dV

Thermal

a = k*dot(grad(T), grad(v))*dx

which corresponds to

K=\int (\nabla N)^Tk(\nabla N)\,dV

Electrostatics

a = epsilon*dot(grad(V), grad(v))*dx

Maxwell

a = inner(curl(A), curl(v))*dx

FEniCS automatically generates the element matrices.


---

2. [FEniCSx (DOLFINx)](https://fenicsproject.org/download/?utm_source=chatgpt.com)

GitHub:

[DOLFINx Repository](https://github.com/FEniCS/dolfinx?utm_source=chatgpt.com)

Modern version of FEniCS.

Supports:

Structural

Thermal

CFD

Electromagnetics

Multiphysics


The generated C++ kernels contain explicit quadrature loops and local element matrices.


---

3. [SfePy (Simple Finite Elements in Python)](https://sfepy.org/?utm_source=chatgpt.com)

GitHub:

[SfePy GitHub](https://github.com/sfepy/sfepy?utm_source=chatgpt.com)

Pure Python FEM.

Examples:

Linear Elasticity

dw_lin_elastic

implements

\int B^TDB\,dV

Heat Conduction

dw_laplace

implements

\int (\nabla N)^Tk(\nabla N)\,dV

This is one of the easiest codebases to read.


---

4. [scikit-fem](https://scikit-fem.readthedocs.io/?utm_source=chatgpt.com)

GitHub:

[scikit-fem GitHub](https://github.com/kinnala/scikit-fem?utm_source=chatgpt.com)

Extremely compact Python FEM.

Example:

@BilinearForm
def laplace(u,v,w):
    return dot(grad(u), grad(v))

which becomes

K=\int \nabla N^T \nabla N\,dV

Great for learning assembly.


---

5. [FiPy](https://www.ctcms.nist.gov/fipy/?utm_source=chatgpt.com)

GitHub:

[FiPy GitHub](https://github.com/usnistgov/fipy?utm_source=chatgpt.com)

Finite-volume Python solver.

Used for:

Heat transfer

Diffusion

Advection

Species transport


Example:

TransientTerm() == DiffusionTerm()


---

6. [OpenFOAM Python Interface (PyFoam)](https://pyfoam.sourceforge.net/?utm_source=chatgpt.com)

GitHub:

[PyFoam GitHub](https://github.com/takaakiaoki/PyFoam?utm_source=chatgpt.com)

Works with OpenFOAM.

Python controls OpenFOAM simulations, but the actual matrix assembly is C++ finite-volume code.


---

7. [PyElmer](https://github.com/nemocrys/pyelmer?utm_source=chatgpt.com)

Python interface for Elmer.

Supports:

Structural

Thermal

CFD

Electromagnetics



---

Best Repositories to Study Actual Numerical Element Matrices

Easiest

1. [scikit-fem GitHub](https://github.com/kinnala/scikit-fem?utm_source=chatgpt.com)


2. [SfePy GitHub](https://github.com/sfepy/sfepy?utm_source=chatgpt.com)



Research/Industrial

3. [FEniCS GitHub Organization](https://github.com/FEniCS?utm_source=chatgpt.com)


4. [DOLFINx Repository](https://github.com/FEniCS/dolfinx?utm_source=chatgpt.com)


5. [Elmer FEM GitHub](https://github.com/ElmerCSC/elmerfem?utm_source=chatgpt.com)



CFD

6. [OpenFOAM Official Site](https://openfoam.org/?utm_source=chatgpt.com)


7. [PyFoam GitHub](https://github.com/takaakiaoki/PyFoam?utm_source=chatgpt.com)



For understanding the actual numerical assembly of , , convection, diffusion, Maxwell curl-curl, and Navier–Stokes matrices, scikit-fem and SfePy are usually the easiest because the entire element formulation can often be understood from a few hundred lines of Python rather than thousands of lines of C++.




