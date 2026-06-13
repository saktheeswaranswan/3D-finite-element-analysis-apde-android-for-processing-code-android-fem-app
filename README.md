# warning 
# written with AI in apde there could be errors 
# these are elements in structural for speed , convergence,accuracy



# FEM B-Matrix and D-Matrix Reference

## Overview

The strain-displacement matrix (B) relates nodal displacements to strains:

ε = B u

The constitutive matrix (D) relates strains to stresses:

σ = D ε

The element stiffness matrix is:

K = ∫ Bᵀ D B dΩ

---

# 1D BAR ELEMENT

## Degrees of Freedom

[u₁ u₂]ᵀ

## B Matrix

B =

[ -1/L   1/L ]

Size: 1×2

## D Matrix

D =

[ E ]

Size: 1×1

## Stiffness

K = (AE/L)

[  1  -1 ]
[ -1   1 ]

---

# EULER-BERNOULLI BEAM

## DOF

[v₁ θ₁ v₂ θ₂]ᵀ

## Curvature Matrix

B =

[ N₁''  N₂''  N₃''  N₄'' ]

Size: 1×4

## D Matrix

D =

[ EI ]

Size: 1×1

## Stiffness

K = ∫ Bᵀ D B dx

---

# CST TRIANGLE (3 NODE)

## DOF

[u₁ v₁ u₂ v₂ u₃ v₃]ᵀ

## B Matrix

B = (1/2A)

[ b₁  0   b₂  0   b₃  0 ]
[ 0   c₁  0   c₂  0   c₃ ]
[ c₁  b₁ c₂  b₂ c₃  b₃ ]

Size: 3×6

## Plane Stress D

D = E/(1-ν²)

[ 1   ν     0 ]
[ ν   1     0 ]
[ 0   0 (1-ν)/2 ]

Size: 3×3

---

# LST TRIANGLE (6 NODE)

## DOF

12

## B Matrix

B =

[ N₁,x 0 ... N₆,x 0 ]
[ 0 N₁,y ... 0 N₆,y ]
[ N₁,y N₁,x ... N₆,y N₆,x ]

Size: 3×12

## D Matrix

Same as CST.

## Stiffness

K = ∫ Bᵀ D B dA

---

# Q4 QUADRILATERAL

## DOF

8

## B Matrix

B =

[ N₁,x 0 N₂,x 0 N₃,x 0 N₄,x 0 ]
[ 0 N₁,y 0 N₂,y 0 N₃,y 0 N₄,y ]
[ N₁,y N₁,x N₂,y N₂,x N₃,y N₃,x N₄,y N₄,x ]

Size: 3×8

## Plane Stress D

D = E/(1-ν²)

[ 1   ν     0 ]
[ ν   1     0 ]
[ 0   0 (1-ν)/2 ]

## Plane Strain D

D = E/((1+ν)(1-2ν))

[ 1-ν   ν      0 ]
[ ν    1-ν     0 ]
[ 0     0   (1-2ν)/2 ]

---

# Q8 QUADRILATERAL

## DOF

16

## B Matrix

B =

[ N₁,x 0 ... N₈,x 0 ]
[ 0 N₁,y ... 0 N₈,y ]
[ N₁,y N₁,x ... N₈,y N₈,x ]

Size: 3×16

## D Matrix

Same as Q4

## Stiffness

K = ∫ Bᵀ D B dA

---

# TET4 (LINEAR TETRAHEDRON)

## DOF

12

## B Matrix

B = (1/6V)

[ b₁ 0 0 b₂ 0 0 b₃ 0 0 b₄ 0 0 ]
[ 0 c₁ 0 0 c₂ 0 0 c₃ 0 0 c₄ 0 ]
[ 0 0 d₁ 0 0 d₂ 0 0 d₃ 0 0 d₄ ]
[ c₁ b₁ 0 c₂ b₂ 0 c₃ b₃ 0 c₄ b₄ 0 ]
[ 0 d₁ c₁ 0 d₂ c₂ 0 d₃ c₃ 0 d₄ c₄ ]
[ d₁ 0 b₁ d₂ 0 b₂ d₃ 0 b₃ d₄ 0 b₄ ]

Size: 6×12

## D Matrix (3D Elasticity)

D = E/((1+ν)(1-2ν))

[1-ν  ν    ν    0 0 0]
[ν   1-ν   ν    0 0 0]
[ν    ν   1-ν   0 0 0]
[0    0    0 (1-2ν)/2 0 0]
[0    0    0 0 (1-2ν)/2 0]
[0    0    0 0 0 (1-2ν)/2]

Size: 6×6

---

# TET10 (SOLID187)

## DOF

30

## B Matrix

B =

[ N₁,x 0 0 ... N₁₀,x 0 0 ]
[ 0 N₁,y 0 ... 0 N₁₀,y 0 ]
[ 0 0 N₁,z ... 0 0 N₁₀,z ]
[ N₁,y N₁,x 0 ... N₁₀,y N₁₀,x 0 ]
[ 0 N₁,z N₁,y ... 0 N₁₀,z N₁₀,y ]
[ N₁,z 0 N₁,x ... N₁₀,z 0 N₁₀,x ]

Size: 6×30

## D Matrix

Same as TET4

---

# HEX8 (SOLID185)

## DOF

24

## B Matrix

B =

[ N₁,x 0 0 ... N₈,x 0 0 ]
[ 0 N₁,y 0 ... 0 N₈,y 0 ]
[ 0 0 N₁,z ... 0 0 N₈,z ]
[ N₁,y N₁,x 0 ... N₈,y N₈,x 0 ]
[ 0 N₁,z N₁,y ... 0 N₈,z N₈,y ]
[ N₁,z 0 N₁,x ... N₈,z 0 N₈,x ]

Size: 6×24

## D Matrix

Same as TET4

---

# HEX20 (SOLID186)

## DOF

60

## B Matrix

B =

[ N₁,x 0 0 ... N₂₀,x 0 0 ]
[ 0 N₁,y 0 ... 0 N₂₀,y 0 ]
[ 0 0 N₁,z ... 0 0 N₂₀,z ]
[ N₁,y N₁,x 0 ... N₂₀,y N₂₀,x 0 ]
[ 0 N₁,z N₁,y ... 0 N₂₀,z N₂₀,y ]
[ N₁,z 0 N₁,x ... N₂₀,z 0 N₂₀,x ]

Size: 6×60

## D Matrix

Same as TET4

---

# HEX27

## DOF

81

## B Matrix

B =

[ N₁,x 0 0 ... N₂₇,x 0 0 ]
[ 0 N₁,y 0 ... 0 N₂₇,y 0 ]
[ 0 0 N₁,z ... 0 0 N₂₇,z ]
[ N₁,y N₁,x 0 ... N₂₇,y N₂₇,x 0 ]
[ 0 N₁,z N₁,y ... 0 N₂₇,z N₂₇,y ]
[ N₁,z 0 N₁,x ... N₂₇,z 0 N₂₇,x ]

Size: 6×81

## D Matrix

Same as TET4

---

# Universal FEM Stiffness Equation

K = ∫ Bᵀ D B dΩ

For numerical integration:

K = Σ(Bgᵀ D Bg |Jg| wg)

where

Bg  = B matrix at Gauss point g
Jg  = Jacobian determinant
wg  = Gauss weight

This formulation is the basis of:

- ANSYS SOLID185
- ANSYS SOLID186
- ANSYS SOLID187
- Abaqus C3D8
- Abaqus C3D20
- Abaqus C3D10
- Nastran CHEXA
- Nastran CTETRA
- CalculiX
- Code_Aster



Giving the full B-matrix for every element (BAR, BEAM, CST, LST, Q4, Q8, TET4, TET10, HEX8, HEX20, HEX27) in one response would be extremely long because:

Q8 → B is 

TET10 → B is 

HEX20 → B is 

HEX27 → B is 


and each entry contains derivatives of shape functions.

The good news is that the D matrix is common for many element families, and the B matrix follows a standard pattern.

1D BAR

B Matrix

B=
\begin{bmatrix}
-\frac1L & \frac1L
\end{bmatrix}

Size:

1\times2

D Matrix

D=
[E]

Size:

1\times1


---

EULER–BERNOULLI BEAM

Curvature Matrix

B=
\begin{bmatrix}
N_1'' & N_2'' & N_3'' & N_4''
\end{bmatrix}

D Matrix

D=
[EI]


---

CST TRIANGLE

B Matrix

B=
\frac1{2A}
\begin{bmatrix}
b_1&0&b_2&0&b_3&0\\
0&c_1&0&c_2&0&c_3\\
c_1&b_1&c_2&b_2&c_3&b_3
\end{bmatrix}

Size:

3\times6

Plane Stress D

D=
\frac{E}{1-\nu^2}
\begin{bmatrix}
1&\nu&0\\
\nu&1&0\\
0&0&\frac{1-\nu}{2}
\end{bmatrix}


---

LST TRIANGLE (6 Node)

B Matrix

B=
\begin{bmatrix}
N_{1,x}&0&\cdots&N_{6,x}&0\\
0&N_{1,y}&\cdots&0&N_{6,y}\\
N_{1,y}&N_{1,x}&\cdots&N_{6,y}&N_{6,x}
\end{bmatrix}

Size:

3\times12

D Matrix

Same as CST.


---

Q4 QUAD

B Matrix

B=
\begin{bmatrix}
N_{1,x}&0&N_{2,x}&0&N_{3,x}&0&N_{4,x}&0\\
0&N_{1,y}&0&N_{2,y}&0&N_{3,y}&0&N_{4,y}\\
N_{1,y}&N_{1,x}&N_{2,y}&N_{2,x}&N_{3,y}&N_{3,x}&N_{4,y}&N_{4,x}
\end{bmatrix}

Size:

3\times8

D Matrix

Plane stress:

D=
\frac{E}{1-\nu^2}
\begin{bmatrix}
1&\nu&0\\
\nu&1&0\\
0&0&\frac{1-\nu}{2}
\end{bmatrix}

Plane strain:

D=
\frac{E}{(1+\nu)(1-2\nu)}
\begin{bmatrix}
1-\nu&\nu&0\\
\nu&1-\nu&0\\
0&0&\frac{1-2\nu}{2}
\end{bmatrix}


---

Q8 QUAD

B Matrix

B=
\begin{bmatrix}
N_{1,x}&0&\cdots&N_{8,x}&0\\
0&N_{1,y}&\cdots&0&N_{8,y}\\
N_{1,y}&N_{1,x}&\cdots&N_{8,y}&N_{8,x}
\end{bmatrix}

Size:

3\times16

D Matrix

Same as Q4.


---

TET4

B Matrix

B=
\frac1{6V}
\begin{bmatrix}
b_1&0&0&b_2&0&0&b_3&0&0&b_4&0&0\\
0&c_1&0&0&c_2&0&0&c_3&0&0&c_4&0\\
0&0&d_1&0&0&d_2&0&0&d_3&0&0&d_4\\
c_1&b_1&0&c_2&b_2&0&c_3&b_3&0&c_4&b_4&0\\
0&d_1&c_1&0&d_2&c_2&0&d_3&c_3&0&d_4&c_4\\
d_1&0&b_1&d_2&0&b_2&d_3&0&b_3&d_4&0&b_4
\end{bmatrix}

Size:

6\times12

D Matrix (3D)

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


---

TET10

B Matrix

B=
\begin{bmatrix}
N_{1,x}&0&0&\cdots&N_{10,x}&0&0\\
0&N_{1,y}&0&\cdots&0&N_{10,y}&0\\
0&0&N_{1,z}&\cdots&0&0&N_{10,z}\\
N_{1,y}&N_{1,x}&0&\cdots&N_{10,y}&N_{10,x}&0\\
0&N_{1,z}&N_{1,y}&\cdots&0&N_{10,z}&N_{10,y}\\
N_{1,z}&0&N_{1,x}&\cdots&N_{10,z}&0&N_{10,x}
\end{bmatrix}

Size:

6\times30

D Matrix

Same 3D elasticity matrix as TET4.


---

HEX8

B Matrix

B=
\begin{bmatrix}
N_{1,x}&0&0&\cdots&N_{8,x}&0&0\\
0&N_{1,y}&0&\cdots&0&N_{8,y}&0\\
0&0&N_{1,z}&\cdots&0&0&N_{8,z}\\
N_{1,y}&N_{1,x}&0&\cdots&N_{8,y}&N_{8,x}&0\\
0&N_{1,z}&N_{1,y}&\cdots&0&N_{8,z}&N_{8,y}\\
N_{1,z}&0&N_{1,x}&\cdots&N_{8,z}&0&N_{8,x}
\end{bmatrix}

Size:

6\times24

D Matrix

Same 3D elasticity matrix.


---

HEX20

B Matrix

B=
\begin{bmatrix}
N_{1,x}&0&0&\cdots&N_{20,x}&0&0\\
0&N_{1,y}&0&\cdots&0&N_{20,y}&0\\
0&0&N_{1,z}&\cdots&0&0&N_{20,z}\\
N_{1,y}&N_{1,x}&0&\cdots&N_{20,y}&N_{20,x}&0\\
0&N_{1,z}&N_{1,y}&\cdots&0&N_{20,z}&N_{20,y}\\
N_{1,z}&0&N_{1,x}&\cdots&N_{20,z}&0&N_{20,x}
\end{bmatrix}

Size:

6\times60

D Matrix

Same 3D elasticity matrix.


---

HEX27

B Matrix

B=
\begin{bmatrix}
N_{1,x}&0&0&\cdots&N_{27,x}&0&0\\
0&N_{1,y}&0&\cdots&0&N_{27,y}&0\\
0&0&N_{1,z}&\cdots&0&0&N_{27,z}\\
N_{1,y}&N_{1,x}&0&\cdots&N_{27,y}&N_{27,x}&0\\
0&N_{1,z}&N_{1,y}&\cdots&0&N_{27,z}&N_{27,y}\\
N_{1,z}&0&N_{1,x}&\cdots&N_{27,z}&0&N_{27,x}
\end{bmatrix}

Size:

6\times81

D Matrix

Same 3D isotropic elasticity matrix.

This is the complete canonical - formulation used in standard FEM derivations. The only remaining element-specific detail is the explicit shape-function derivatives , which differ between TET10, HEX8, HEX20, and HEX27.
