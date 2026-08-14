# European Option Pricing: Black-Scholes vs. PDE Finite Difference Methods

A quantitative finance project comparing European Call option pricing using three distinct approaches:
1. **Analytical Closed-Form** (Black-Scholes-Merton)
2. **Explicit Finite Difference Method** (FDM)
3. **Crank-Nicolson Method** (Unconditionally Stable Hybrid FDM)

---

## 🧮 Mathematical Background

### 1. Black-Scholes PDE
The evolution of a European Call option price $V(S,t)$ is governed by:

$$\frac{\partial V}{\partial t} + \frac{1}{2}\sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} + r S \frac{\partial V}{\partial S} - rV = 0$$

### 2. Numerical Schemes
* **Explicit Scheme:** Forward-difference in time, central-difference in space. Simple to compute, but subject to strict Courant-Friedrichs-Lewy (CFL) stability bounds on $\Delta t$.
* **Crank-Nicolson Scheme:** A continuous average of implicit and explicit schemes at $n + \frac{1}{2}$. It is unconditionally stable and achieves second-order accuracy in time: $O(\Delta t^2 + \Delta S^2)$.

$$\mathbf{A} V^{n+1} = \mathbf{B} V^n + \mathbf{b}$$

---

## 📊 Method Comparison & Results

*Parameters: $S_0 = 100$, $K = 100$, $T = 1.0$, $r = 0.5\%$, $\sigma = 20\%$*

| Pricing Method | Scheme Type | Calculated Price | Absolute Error | Accuracy Order |
| :--- | :--- | :--- | :--- | :--- |
| **Black-Scholes** | Analytical | `$8.0163$` | Baseline | Exact |
| **Explicit FDM** | Numerical (Explicit) | `$8.0158$` | `~0.0005` | $O(\Delta t + \Delta S^2)$ |
| **Crank-Nicolson** | Numerical (Implicit) | `$8.0161$` | `~0.0002` | $O(\Delta t^2 + \Delta S^2)$ |

---

## 🛠️ Key Quantitative Takeaways

* **Linear System Solver:** The Crank-Nicolson method constructs tridiagonal coefficient matrices $\mathbf{A}$ and $\mathbf{B}$ solved at each time step using `np.linalg.solve`.
* **Boundary Conditions:** Time-dependent boundary conditions applied at $S_{\text{max}}$:
  $$V(S_{\text{max}}, \tau) = S_{\text{max}} - K e^{-r\tau}$$
* **Interpolation:** Linear spatial interpolation (`np.interp`) is used to retrieve precise option values at non-grid spot targets ($S_0 = 100$).

---

## 🚀 How to Run
Open `option_pricing.ipynb` in Jupyter Notebook or VS Code and run all cells.
