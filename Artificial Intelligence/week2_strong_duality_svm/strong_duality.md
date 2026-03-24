Understood. Here is the strict mathematical proof of why strong duality holds, using the Separating Hyperplane Theorem, without analogies.

---

### 1. Mathematical Definitions of the Primal and Dual Problems

First, let's define the standard form of the **Primal problem** for SVM.
Let $x = (w, b)$.
$$p^* = \min_x f(x) \quad \text{subject to} \quad g_i(x) \le 0 \quad (i = 1, \dots, m)$$
* **Objective function:** $f(x) = \frac{1}{2}\|w\|^2$
* **Constraints:** $g_i(x) = 1 - y_i(w^T x_i + b)$
* $p^*$ is the optimal value (minimum) of this Primal problem.

We introduce the Lagrange multipliers $\alpha_i \ge 0$ to form the **Lagrangian**:
$$\mathcal{L}(x, \alpha) = f(x) + \sum_{i=1}^m \alpha_i g_i(x)$$

The **Dual problem** is defined as:
$$d^* = \max_{\alpha \ge 0} \min_x \mathcal{L}(x, \alpha)$$
* $d^*$ is the optimal value (maximum) of the Dual problem.

### 2. Proof of Weak Duality

For any valid $x$ and any $\alpha \ge 0$, the following inequality always holds:
$$\min_{x'} \mathcal{L}(x', \alpha) \le \mathcal{L}(x, \alpha) \le \max_{\alpha' \ge 0} \mathcal{L}(x, \alpha')$$

The leftmost term is the Dual objective function $q(\alpha)$, and the rightmost concept relates to the Primal problem. By taking the maximum over $\alpha$ on the left and the minimum over $x$ on the right, we unconditionally get:
$$d^* \le p^*$$
This is known as **Weak Duality**.

### 3. Constructing the Convex Set for Strong Duality

Now, let's prove geometrically and analytically why $p^* = d^*$.
We define a set of values $\mathcal{A}$ in the domain:
$$\mathcal{A} = \{ (u, t) \in \mathbb{R}^m \times \mathbb{R} \mid \exists x \text{ s.t. } g_i(x) \le u_i, f(x) \le t \}$$

**The Core Concept:** Because $f(x)$ is a quadratic form, it is a **convex function**. Since $g_i(x)$ are affine (linear) equations, they are also convex. Therefore, **the set $\mathcal{A}$ is a Convex Set.**

The Primal optimal value $p^*$ represents the boundary point $(0, p^*)$ of this set $\mathcal{A}$ (i.e., the minimum value of $t$ when $u \le 0$). 
Consequently, there is no point inside set $\mathcal{A}$ where $u \le 0$ and $t < p^*$.



### 4. Applying the Separating Hyperplane Theorem

Let $\mathcal{B}$ be the region strictly below the optimal point: $\mathcal{B} = \{ (0, t) \mid t < p^* \}$.
Since $\mathcal{A}$ is a convex set and it does not intersect with $\mathcal{B}$, the **Separating Hyperplane Theorem** states that there must exist a hyperplane separating these two regions.

Let the normal vector of this hyperplane be $(\lambda, \mu)$. For every point $(u, t)$ in $\mathcal{A}$, the following inequality holds:
$$\sum_{i=1}^m \lambda_i u_i + \mu t \ge \mu p^*$$

Because $u_i$ and $t$ can increase infinitely by the definition of $\mathcal{A}$, the components of the normal vector must be non-negative ($\lambda_i \ge 0$ and $\mu \ge 0$) for the inequality to hold universally.

### 5. Slater's Condition and the Conclusion of Strong Duality

This is where **Slater's Condition** comes into play. Satisfying Slater's condition mathematically guarantees that $\mu > 0$. (If $\mu = 0$, it contradicts the premise that a strictly feasible point exists).

Since $\mu > 0$, we can divide both sides by $\mu$. Let's define $\alpha_i = \lambda_i / \mu$:
$$\sum_{i=1}^m \alpha_i u_i + t \ge p^*$$

By the definition of $\mathcal{A}$, the point where $u_i = g_i(x)$ and $t = f(x)$ also belongs to $\mathcal{A}$. Substituting these in yields:
$$f(x) + \sum_{i=1}^m \alpha_i g_i(x) \ge p^*$$
Notice that the left side is exactly our Lagrangian:
$$\mathcal{L}(x, \alpha) \ge p^*$$

Since this holds for all $x$, we can take the minimum over $x$, and the inequality remains true:
$$\min_x \mathcal{L}(x, \alpha) \ge p^*$$

The left side is now the Dual objective function $q(\alpha)$:
$$q(\alpha) \ge p^*$$

Because the Dual optimal value $d^*$ is the maximum of $q(\alpha)$, it must be true that $d^* \ge q(\alpha)$. Therefore:
$$d^* \ge p^*$$

In Step 2 (Weak Duality), we proved $d^* \le p^*$. Now we have proved $d^* \ge p^*$.
Consequently, the only mathematical possibility is:
**$$d^* = p^*$$ (Proof of Strong Duality completed)**

---

### 💡 Summary for SVM

In SVM, the objective function $\frac{1}{2}\|w\|^2$ is strictly convex, and the constraints $1 - y_i(w^T x_i + b) \le 0$ are affine. 

In convex optimization, when all constraints are affine, a **Refined Slater's Condition** applies. This means that as long as the feasible region is not entirely empty (i.e., the data is linearly separable), it automatically guarantees $\mu > 0$. 

Thus, by the Separating Hyperplane Theorem, the Primal and Dual problems are perfectly identical ($d^* = p^*$). This proves why you can swap `min max` to `max min` without any mathematical error.

Would you like me to show you how we derive the **KKT (Karush-Kuhn-Tucker) conditions** based on this exact duality result?