# European Option Pricing: Black-Scholes vs. PDE Finite Difference Methods

A quantitative finance project comparing European Call option pricing using three distinct approaches:
1. **Analytical Closed-Form** using the Black-Scholes formula
2. **Explicit Finite Difference Method**
3. **Crank-Nicolson Method**



## How it works

### 1. Black-Scholes PDE
The equation for the European Call option price $V(S,t)$ is:

$$\frac{\partial V}{\partial t} + \frac{1}{2}\sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} + r S \frac{\partial V}{\partial S} - rV = 0$$

### 2. Numerical Schemes
* **Explicit Scheme:** Forward-difference in time, central-difference in space. Main constraint is that it is subject to strict Courant-Friedrichs-Lewy (CFL) stability bounds on $\Delta t$.
* **Crank-Nicolson Scheme:** An implicit-explicit hybrid evaluated at $n + \frac{1}{2}$. It is unconditionally stable and achieves second-order accuracy in time: $O(\Delta t^2 + \Delta S^2)$.

$$\mathbf{A} V^{n+1} = \mathbf{B} V^n + \mathbf{b}$$



## Results comparison 

**Parameters:** $S_0 = 100$, $K = 100$, $T = 1.0$, $r = 0.5\%$, $\sigma = 20\%$

| Pricing Method | Scheme Type | Calculated Price | Absolute Error |
| :--- | :--- | :--- | :--- |
| **Black-Scholes** | Analytical | $8.0163 | Baseline |
| **Explicit FDM** | Numerical (Explicit) | $8.0158 | ~0.0005 |
| **Crank-Nicolson** | Numerical (Implicit) | $8.0161 | ~0.0002 |



## Key Design Choices

* **Efficient Linear Solving:** Used `np.linalg.solve` to solve the implicit matrix system $\mathbf{A} V^{n+1} = \mathbf{B} V^n$ at each time step.
* **Dynamic Boundaries:** Enforced time-dependent boundary conditions at $S_{\text{max}}$ to account for option payoff decay over time.
* **Sub-Grid Precision:** Applied linear spatial interpolation (`np.interp`) to evaluate exact option values at non-grid spot targets ($S_0 = 100$).


## How to Run
Open `option_pricing.ipynb` in Jupyter Notebook or VS Code and run all cells.
