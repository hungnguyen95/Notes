# Manhattan Distance
The Manhattan distance between a point $P$ and a line $L$ is defined as the smallest distance between anypoint $M \in L$ on the line and $P$:

$d(P,L) = \min_{M \in L} d(M,P)$

The Manhattan distance between 2 points is defined as:

$d(M,P) = |M_x - P_x| + |M_y - P_y|$

The Manhattan distance also known as $L_1$ norm

The question is then **what is the formula that gives the manhattan distance between a point and a line?**.

**Proposition 1:**  The manhattan distance between a point of coordinates $(x_0, y_0)$ and a line of equation: $ax + by + c = 0$ is given by:

$\frac{|ax_0 + by_0 +c|}{max(|a|, |b|)}$

Since $a$ and $b$ can not be both 0, the formula is legal.