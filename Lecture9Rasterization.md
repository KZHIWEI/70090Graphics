# The Graphics Pipeline

![image-20250110203607508](Lecture9Rasterization.assets/image-20250110203607508.png)

![image-20250110203800168](Lecture9Rasterization.assets/image-20250110203800168.png)

# Rasterization

![image-20250110203840611](Lecture9Rasterization.assets/image-20250110203840611.png)

![image-20250110204123004](Lecture9Rasterization.assets/image-20250110204123004.png)

# Coordinate intuition

## Trilinear coordinates

![image-20250110204226262](Lecture9Rasterization.assets/image-20250110204226262.png)

When interpolation how $a', b',c'$ define the how corresponding vertex weight. Calculating weights or contributions of the opposite vertex.

## barycentric coordinates

![image-20250110204737416](Lecture9Rasterization.assets/image-20250110204737416.png)

In centroid $c$ all three vertexes are equally important. centroid $c$ can be found by connect edges to it's corresponding vertex.

------

## 1. What are barycentric coordinates?

Given a triangle $ABC$, **barycentric coordinates** $(\alpha, \beta, \gamma)$ for a point $P$ satisfy:

1. $\alpha + \beta + \gamma = 1$.
2. $P = \alpha \, A + \beta \, B + \gamma \, C$.

where each of $\alpha,\beta,\gamma$ can be interpreted as a **weight** telling you how much “influence” each vertex $A,B,C$ has on the point $P$.

- If $P$ lies **exactly at** $A$, then the barycentric coordinates would be $(1,0,0)$.
- If $P$ is anywhere on **side $BC$**, then $\alpha = 0$, so the coordinates look like $(0,\beta,\gamma)$.
- If $P$ is **inside** the triangle, then all three weights $\alpha, \beta, \gamma$ are **positive** (but still sum to 1).

In the slides, the triangle’s corners are labeled with barycentric coordinates:

- $A$ at $(1,0,0)$,
- $B$ at $(0,1,0)$,
- $C$ at $(0,0,1)$.

That notation simply means:

- Vertex $A$ is 100% “A,” 0% “B,” 0% “C.”
- Vertex $B$ is 0% “A,” 100% “B,” 0% “C.”
- Vertex $C$ is 0% “A,” 0% “B,” 100% “C.”

Any point $P_1$ inside the triangle has $\alpha,\beta,\gamma$ such that $\alpha + \beta + \gamma = 1$, with each coordinate typically between 0 and 1.

------

## 2. Why they’re called “barycentric”

“Barycentric” comes from the idea of a **center of mass** (a “balance point”). If you imagine the triangle as a thin metal plate with densities proportional to $\alpha,\beta,\gamma$, the point $P_1$ is precisely where the plate would balance if $A,B,C$ were the only mass points with weights $\alpha,\beta,\gamma$.

------

## 3. Geometric intuition

1. **Areas**: One way to interpret $\alpha,\beta,\gamma$ is through **sub-triangle areas**. For instance,
   - $\alpha$ is proportional to the area of the triangle $P_1 B C$.
   - $\beta$ is proportional to the area of the triangle $P_1 A C$.
   - $\gamma$ is proportional to the area of the triangle $P_1 A B$.
2. **Lines**: If you fix one coordinate (say $\alpha$) and let the other two vary, you trace a line parallel to side $BC$ of the triangle.
3. **Interpolations**: Another viewpoint is that moving $P_1$ within the triangle changes $\alpha,\beta,\gamma$ continuously, reflecting how much “weight” each vertex contributes.

------

## 4. Putting coordinates on each vertex

In the slides, each vertex is assigned coordinates in 3D space in a special way:

- A = (1,0,0)
- B = (0,1,0)
- C = (0,0,1)

Then any point inside the triangle can be described as (α,β,γ) with $\alpha + \beta + \gamma = 1$. Visually, you might think of $\alpha,\beta,\gamma$ as the fraction you travel along each axis, but the key is that we’re working in a coordinate system specifically designed so that a “plain” triangle in 2D looks like a simplex in 3D with corners on the unit axes.

------

## 5. Summary

- **Barycentric coordinates** give a convenient way to write a point in a triangle as a combination of the triangle’s vertices.
- They always sum to 1.
- If $\alpha, \beta, \gamma \geq 0$, the point is inside (or on) the triangle.
- Each vertex “anchors” one axis of this barycentric space.

# A triangle in terms of vectors

![image-20250110205015985](Lecture9Rasterization.assets/image-20250110205015985.png)

![image-20250110205043740](Lecture9Rasterization.assets/image-20250110205043740.png)

- collinear : 共线

# Basis vectors

![image-20250110210003868](Lecture9Rasterization.assets/image-20250110210003868.png)

![image-20250110210033187](Lecture9Rasterization.assets/image-20250110210033187.png)

# Barycentric coordinates

![image-20250110210130097](Lecture9Rasterization.assets/image-20250110210130097.png)

![image-20250110210206319](Lecture9Rasterization.assets/image-20250110210206319.png)

# Barycentric coordinates and signed distances

![image-20250110210446454](Lecture9Rasterization.assets/image-20250110210446454.png)

- **Definition**: In **barycentric coordinates**, any point $p$ in the plane of triangle $ABC$ can be written as

  $p \;=\; \alpha\,a \;+\; \beta\,b \;+\; \gamma\,c$,

  where $\alpha + \beta + \gamma = 1$.

- **Signed Distance Interpretation**: Each coordinate (e.g., $\beta$) is proportional to the signed distance of $p$ from the line *opposite* the corresponding vertex. In the slide’s example, $\beta$ measures the signed distance from $p$ to line $AC$.

- **Edges as Zero-Level Sets**:

  - $\alpha = 0$ corresponds to the line BC.
  - $\beta$ = 0 corresponds to the line $AC$.
  - $\gamma = 0$ corresponds to the line $AB$.
     Points on each edge satisfy the condition that the associated barycentric coordinate is zero.

- **Normalization**: Barycentric coordinates are often normalized so that $\alpha + \beta + \gamma = 1$, ensuring they sum to unity. This also conveniently aligns with area ratios when the coordinates are nonnegative.

- **Geometric/Rendering Applications**:

  - **Interpolation**: Useful for interpolating attributes (color, texture coordinates) within a triangle in computer graphics (rasterization).
  - **Triangle Mesh**: Barycentric coordinates make it easy to determine whether a point lies inside (all coordinates $\geq 0$) or outside (at least one negative coordinate) the triangle.

![image-20250110210816875](Lecture9Rasterization.assets/image-20250110210816875.png)

![image-20250110210852569](Lecture9Rasterization.assets/image-20250110210852569.png)

# Implicit equation for lines

![image-20250110210944728](Lecture9Rasterization.assets/image-20250110210944728.png)

This equation represents the general line equation passing through two points (xa,ya)(x_a, y_a) and (xb,yb)(x_b, y_b).

### Breakdown of the Equation:

1. **Line Equation Format**:
    It is written in the form $Ax + By + C = 0$, where:
   - $A = y_a - y_b$
   - $B = x_b - x_a$
   - $C = x_a y_b - x_b y_a$
2. **Derivation**:
   - The slope of the line through points $(x_a, y_a)$ and $(x_b, y_b)$ is given by: $m = \frac{y_b - y_a}{x_b - x_a}$.
   - Using the point-slope form, we arrive at this general form for the line equation.
3. **Geometric Meaning**:
   - For any point $(x, y)$ on the line, this equation holds true.
   - The coefficients $A$, $B$, and $C$ are determined by the coordinates of the two points defining the line.
4. **Special Cases**:
   - If $x_a = x_b$, the line is vertical, and the equation simplifies to $x = x_a$.
   - If $y_a = y_b$, the line is horizontal, and the equation simplifies to $y = y_a$.

$$
\begin{align}
A &= (x_a,y_a) \\
B &= (x_b,y_b) \\
m &= \frac{y_b-y_a}{x_b-x_a}\\
mx+c &= y\\
\frac{y_b-y_a}{x_b-x_a} x_a+c &= y_a\\
c &= y_a - \frac{y_b-y_a}{x_b-x_a}x_a\\
y &= \frac{y_b-y_a}{x_b-x_a}x+y_a-\frac{y_b-y_a}{x_b-x_a}x_a\\
(x_b-x_a)y &= (y_b-y_a)x+ (x_b-x_a)y_a - (y_b-y_a)x_a\\
(y_a-y_b)x + (x_b-x_a)y &= (x_b-x_a)y_a - (y_b-y_a)x_a \\
(y_a-y_b)x + (x_b-x_a)y &= x_b y_a -x_a y_a - y_b x_a + y_a x_a\\
(y_a-y_b)x + (x_b-x_a)y &= x_b y_a -  y_b x_a \\
(y_a-y_b)x + (x_b-x_a)y - x_b y_a + y_b x_a &= 0
\end{align}
$$



![image-20250110211628540](Lecture9Rasterization.assets/image-20250110211628540.png)

# Edge equations

![image-20250110211649129](Lecture9Rasterization.assets/image-20250110211649129.png)

- $f_{ab}$ : $\gamma = 0$
- $f_{bc}$: $\alpha = 0$
- $f_{ca}$ : $\beta = 0$

![image-20250110213407376](Lecture9Rasterization.assets/image-20250110213407376.png)

![image-20250110213624580](Lecture9Rasterization.assets/image-20250110213624580.png)

![image-20250110213836932](Lecture9Rasterization.assets/image-20250110213836932.png)

![image-20250110215354686](Lecture9Rasterization.assets/image-20250110215354686.png)

![image-20250110215438350](Lecture9Rasterization.assets/image-20250110215438350.png)

# Triangle Rasterization

![image-20250110215551926](Lecture9Rasterization.assets/image-20250110215551926.png)

![image-20250110215755526](Lecture9Rasterization.assets/image-20250110215755526.png)

$c0, c1$ and $c2$ are vertex color

# Visibility: One triangle

![image-20250110215934149](Lecture9Rasterization.assets/image-20250110215934149.png)

# Hidden Surface Removal

![image-20250110220027261](Lecture9Rasterization.assets/image-20250110220027261.png)

# Visibility: Two triangles

![image-20250110220109398](Lecture9Rasterization.assets/image-20250110220109398.png)

# Visibility: Pixels vs Fragments

![image-20250110220241141](Lecture9Rasterization.assets/image-20250110220241141.png)

# Visibility: Which triangle should be drawn first?

![image-20250110220255440](Lecture9Rasterization.assets/image-20250110220255440.png)

![image-20250110220416688](Lecture9Rasterization.assets/image-20250110220416688.png)

# Painter’s Algorithm

![image-20250110220431515](Lecture9Rasterization.assets/image-20250110220431515.png)

# Painter’s Algorithm Problem

![image-20250110220539527](Lecture9Rasterization.assets/image-20250110220539527.png)

# Depth Buffer (z-Buffer)

![image-20250110220626965](Lecture9Rasterization.assets/image-20250110220626965.png)

![image-20250110220816377](Lecture9Rasterization.assets/image-20250110220816377.png)

![image-20250110220824565](Lecture9Rasterization.assets/image-20250110220824565.png)

![image-20250110220836417](Lecture9Rasterization.assets/image-20250110220836417.png)

![image-20250110220847735](Lecture9Rasterization.assets/image-20250110220847735.png)

# The Z-Buffer Algorithm

![image-20250110220932917](Lecture9Rasterization.assets/image-20250110220932917.png)

# Z-buffer Algorithm Properties

![image-20250110221110858](Lecture9Rasterization.assets/image-20250110221110858.png)

# Alias Effects

![image-20250110221158829](Lecture9Rasterization.assets/image-20250110221158829.png)

# Alias Effects at straight boundaries in raster images.

![image-20250110221344638](Lecture9Rasterization.assets/image-20250110221344638.png)

![image-20250110221430988](Lecture9Rasterization.assets/image-20250110221430988.png)

# Supersampling

![image-20250110221750195](Lecture9Rasterization.assets/image-20250110221750195.png)

![image-20250110221759186](Lecture9Rasterization.assets/image-20250110221759186.png)

![image-20250110222035095](Lecture9Rasterization.assets/image-20250110222035095.png)

- above the line $I_2$ below the line $I_1$

# Limitations of Supersampling

![image-20250110222235864](Lecture9Rasterization.assets/image-20250110222235864.png)

![image-20250110222246836](Lecture9Rasterization.assets/image-20250110222246836.png)

A visualization of the actual pixel intensities after averaging, where the line's intensity gets distributed unevenly, particularly challenging for thin lines.

# Convolution filtering

![image-20250110222510573](Lecture9Rasterization.assets/image-20250110222510573.png)

![image-20250110222651929](Lecture9Rasterization.assets/image-20250110222651929.png)

![image-20250110222708484](Lecture9Rasterization.assets/image-20250110222708484.png)

# Weighted averages

![image-20250110222726451](Lecture9Rasterization.assets/image-20250110222726451.png)

![image-20250110222739678](Lecture9Rasterization.assets/image-20250110222739678.png)

![image-20250110222749242](Lecture9Rasterization.assets/image-20250110222749242.png)

# Pros and Cons of Convolution filtering

![image-20250110222900012](Lecture9Rasterization.assets/image-20250110222900012.png)

# Anti-Aliasing textures

![image-20250110222945173](Lecture9Rasterization.assets/image-20250110222945173.png)

![image-20250110222954430](Lecture9Rasterization.assets/image-20250110222954430.png)