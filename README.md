# Finite Element Method (FEM)
# Canonical B-Matrices, D-Matrices, Shape Functions and Stiffness Formulations

---

# General Linear Elastic FEM Formulation

For any finite element:

\[
K_e
=
\int_{\Omega_e}
B^T D B
\,d\Omega
\]

Numerical integration:

\[
K_e
=
\sum_{g=1}^{n_g}
B_g^T
D
B_g
|J_g|
w_g
\]

where

| Symbol | Meaning |
|----------|----------|
| \(K_e\) | Element stiffness matrix |
| \(B\) | Strain-displacement matrix |
| \(D\) | Constitutive matrix |
| \(J\) | Jacobian |
| \(w\) | Gauss weight |
| \(\Omega\) | Element domain |

---

# Fundamental FEM Relationship

\[
\varepsilon
=
B u
\]

\[
\sigma
=
D \varepsilon
\]

\[
K_e
=
\int B^T D B\,d\Omega
\]

---

# 1D BAR ELEMENT

## Nodes

2

## DOF

\[
[u_1,u_2]
\]

## Shape Functions

\[
N_1
=
1-\frac{x}{L}
\]

\[
N_2
=
\frac{x}{L}
\]

## Displacement Field

\[
u
=
N_1u_1+N_2u_2
\]

## B Matrix

\[
B
=
\begin{bmatrix}
-\frac1L &
\frac1L
\end{bmatrix}
\]

Size:

\[
1\times2
\]

## D Matrix

\[
D=[E]
\]

Size:

\[
1\times1
\]

## Stiffness Matrix

\[
K
=
\frac{AE}{L}
\begin{bmatrix}
1 & -1\\
-1 & 1
\end{bmatrix}
\]

---

# EULER-BERNOULLI BEAM

## Nodes

2

## DOF

\[
[v_1,\theta_1,v_2,\theta_2]
\]

## Cubic Hermite Shape Functions

\[
N_1
=
1-3\xi^2+2\xi^3
\]

\[
N_2
=
L(\xi-2\xi^2+\xi^3)
\]

\[
N_3
=
3\xi^2-2\xi^3
\]

\[
N_4
=
L(-\xi^2+\xi^3)
\]

## Curvature Matrix

\[
B
=
\begin{bmatrix}
N_1''&
N_2''&
N_3''&
N_4''
\end{bmatrix}
\]

## Material Matrix

\[
D=[EI]
\]

## Stiffness Matrix

\[
K=
\frac{EI}{L^3}
\begin{bmatrix}
12&6L&-12&6L\\
6L&4L^2&-6L&2L^2\\
-12&-6L&12&-6L\\
6L&2L^2&-6L&4L^2
\end{bmatrix}
\]

---

# CST TRIANGLE

(Constant Strain Triangle)

## Shape Functions

\[
N_i
=
\frac{a_i+b_ix+c_iy}{2A}
\]

\[
i=1,2,3
\]

## B Matrix

\[
B=
\frac1{2A}
\begin{bmatrix}
b_1&0&b_2&0&b_3&0\\
0&c_1&0&c_2&0&c_3\\
c_1&b_1&c_2&b_2&c_3&b_3
\end{bmatrix}
\]

Size:

\[
3\times6
\]

## Plane Stress D

\[
D=
\frac{E}{1-\nu^2}
\begin{bmatrix}
1&\nu&0\\
\nu&1&0\\
0&0&\frac{1-\nu}{2}
\end{bmatrix}
\]

## Plane Strain D

\[
D=
\frac{E}
{(1+\nu)(1-2\nu)}
\begin{bmatrix}
1-\nu&\nu&0\\
\nu&1-\nu&0\\
0&0&\frac{1-2\nu}{2}
\end{bmatrix}
\]

---

# LST TRIANGLE (6 NODE)

## Area Coordinates

\[
L_1+L_2+L_3=1
\]

## Shape Functions

\[
N_1=L_1(2L_1-1)
\]

\[
N_2=L_2(2L_2-1)
\]

\[
N_3=L_3(2L_3-1)
\]

\[
N_4=4L_1L_2
\]

\[
N_5=4L_2L_3
\]

\[
N_6=4L_3L_1
\]

## B Matrix

\[
B=
\begin{bmatrix}
N_{1,x}&0&\cdots&N_{6,x}&0\\
0&N_{1,y}&\cdots&0&N_{6,y}\\
N_{1,y}&N_{1,x}&\cdots&N_{6,y}&N_{6,x}
\end{bmatrix}
\]

Size:

\[
3\times12
\]

---

# Q4 QUADRILATERAL

## Natural Coordinates

\[
(\xi,\eta)
\]

## Shape Functions

\[
N_1
=
\frac14(1-\xi)(1-\eta)
\]

\[
N_2
=
\frac14(1+\xi)(1-\eta)
\]

\[
N_3
=
\frac14(1+\xi)(1+\eta)
\]

\[
N_4
=
\frac14(1-\xi)(1+\eta)
\]

## B Matrix

\[
B=
\begin{bmatrix}
N_{1,x}&0&\cdots&N_{4,x}&0\\
0&N_{1,y}&\cdots&0&N_{4,y}\\
N_{1,y}&N_{1,x}&\cdots&N_{4,y}&N_{4,x}
\end{bmatrix}
\]

Size:

\[
3\times8
\]

---

# Q8 SERENDIPITY ELEMENT

## Nodes

8

## Shape Functions

Corner node:

\[
N_i
=
\frac14
(1+\xi_i\xi)
(1+\eta_i\eta)
(\xi_i\xi+\eta_i\eta-1)
\]

Mid-side node:

\[
N_i
=
\frac12
(1-\xi^2)
(1+\eta_i\eta)
\]

or

\[
N_i
=
\frac12
(1-\eta^2)
(1+\xi_i\xi)
\]

## B Matrix

\[
B=
\begin{bmatrix}
N_{1,x}&0&\cdots&N_{8,x}&0\\
0&N_{1,y}&\cdots&0&N_{8,y}\\
N_{1,y}&N_{1,x}&\cdots&N_{8,y}&N_{8,x}
\end{bmatrix}
\]

Size:

\[
3\times16
\]

---

# TET4

## Shape Functions

\[
N_i
=
\frac{a_i+b_ix+c_iy+d_iz}{6V}
\]

## B Matrix

\[
B=
\frac1{6V}
\begin{bmatrix}
b_1&0&0&\cdots&b_4&0&0\\
0&c_1&0&\cdots&0&c_4&0\\
0&0&d_1&\cdots&0&0&d_4\\
c_1&b_1&0&\cdots&c_4&b_4&0\\
0&d_1&c_1&\cdots&0&d_4&c_4\\
d_1&0&b_1&\cdots&d_4&0&b_4
\end{bmatrix}
\]

Size:

\[
6\times12
\]

---

# TET10

## Quadratic Shape Functions

\[
N_i=L_i(2L_i-1)
\]

Corner nodes

\[
N_{ij}=4L_iL_j
\]

Edge nodes

## B Matrix

\[
B=
\begin{bmatrix}
N_{1,x}&0&0&\cdots&N_{10,x}&0&0\\
0&N_{1,y}&0&\cdots&0&N_{10,y}&0\\
0&0&N_{1,z}&\cdots&0&0&N_{10,z}\\
N_{1,y}&N_{1,x}&0&\cdots&N_{10,y}&N_{10,x}&0\\
0&N_{1,z}&N_{1,y}&\cdots&0&N_{10,z}&N_{10,y}\\
N_{1,z}&0&N_{1,x}&\cdots&N_{10,z}&0&N_{10,x}
\end{bmatrix}
\]

Size:

\[
6\times30
\]

---

# HEX8 (SOLID185)

## Shape Functions

\[
N_i
=
\frac18
(1+\xi_i\xi)
(1+\eta_i\eta)
(1+\zeta_i\zeta)
\]

## B Matrix

\[
B=
\begin{bmatrix}
N_{1,x}&0&0&\cdots&N_{8,x}&0&0\\
0&N_{1,y}&0&\cdots&0&N_{8,y}&0\\
0&0&N_{1,z}&\cdots&0&0&N_{8,z}\\
N_{1,y}&N_{1,x}&0&\cdots&N_{8,y}&N_{8,x}&0\\
0&N_{1,z}&N_{1,y}&\cdots&0&N_{8,z}&N_{8,y}\\
N_{1,z}&0&N_{1,x}&\cdots&N_{8,z}&0&N_{8,x}
\end{bmatrix}
\]

Size:

\[
6\times24
\]

---

# HEX20 (SOLID186)

## Quadratic Shape Functions

Corner node:

\[
N_i=
\frac18
(1+\xi_i\xi)
(1+\eta_i\eta)
(1+\zeta_i\zeta)
(\xi_i\xi+\eta_i\eta+\zeta_i\zeta-2)
\]

Edge node example:

\[
N=
\frac14
(1-\xi^2)
(1+\eta_i\eta)
(1+\zeta_i\zeta)
\]

## B Matrix

\[
B
=
\begin{bmatrix}
N_{1,x}&0&0&\cdots&N_{20,x}&0&0\\
0&N_{1,y}&0&\cdots&0&N_{20,y}&0\\
0&0&N_{1,z}&\cdots&0&0&N_{20,z}\\
N_{1,y}&N_{1,x}&0&\cdots&N_{20,y}&N_{20,x}&0\\
0&N_{1,z}&N_{1,y}&\cdots&0&N_{20,z}&N_{20,y}\\
N_{1,z}&0&N_{1,x}&\cdots&N_{20,z}&0&N_{20,x}
\end{bmatrix}
\]

Size:

\[
6\times60
\]

---

# HEX27

## Complete Quadratic Brick

1D quadratic basis:

\[
L_1(\xi)=\frac12\xi(\xi-1)
\]

\[
L_2(\xi)=1-\xi^2
\]

\[
L_3(\xi)=\frac12\xi(\xi+1)
\]

Shape functions:

\[
N_{ijk}
=
L_i(\xi)L_j(\eta)L_k(\zeta)
\]

Total:

\[
27
\]

shape functions

## B Matrix

\[
B
=
\begin{bmatrix}
N_{1,x}&0&0&\cdots&N_{27,x}&0&0\\
0&N_{1,y}&0&\cdots&0&N_{27,y}&0\\
0&0&N_{1,z}&\cdots&0&0&N_{27,z}\\
N_{1,y}&N_{1,x}&0&\cdots&N_{27,y}&N_{27,x}&0\\
0&N_{1,z}&N_{1,y}&\cdots&0&N_{27,z}&N_{27,y}\\
N_{1,z}&0&N_{1,x}&\cdots&N_{27,z}&0&N_{27,x}
\end{bmatrix}
\]

Size:

\[
6\times81
\]

---

# General Anisotropic Material Matrix

Most General Linear Elastic Constitutive Law

\[
\sigma=D\varepsilon
\]

\[
D=
\begin{bmatrix}
C_{11}&C_{12}&C_{13}&C_{14}&C_{15}&C_{16}\\
C_{12}&C_{22}&C_{23}&C_{24}&C_{25}&C_{26}\\
C_{13}&C_{23}&C_{33}&C_{34}&C_{35}&C_{36}\\
C_{14}&C_{24}&C_{34}&C_{44}&C_{45}&C_{46}\\
C_{15}&C_{25}&C_{35}&C_{45}&C_{55}&C_{56}\\
C_{16}&C_{26}&C_{36}&C_{46}&C_{56}&C_{66}
\end{bmatrix}
\]

Independent Constants:

\[
21
\]

---

# Important Conclusion

For every FEM element:

\[
\varepsilon=Bu
\]

\[
\sigma=D\varepsilon
\]

\[
K_e=\int B^TDB\,d\Omega
\]

The:

- Shape functions define \(N\)
- Derivatives define \(B\)
- Material law defines \(D\)

Changing isotropic → orthotropic → transversely isotropic → anisotropic changes only \(D\).

The element interpolation and \(B\)-matrix formulation remain unchanged.
