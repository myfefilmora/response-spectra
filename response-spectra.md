# Newmark-Beta Method for Calculating Spectral Acceleration

This guide walks through the steps to compute the spectral acceleration for a given period using the Newmark-beta method.

## Problem Setup
- **Mass (m):** Assume `m = 1 kg`.
- **Damping Ratio (ζ):** Assume `ζ = 0.05` (5% damping).
- **Natural Period (T):** Assume `T = 1 second`.
- **Ground Acceleration (\( \ddot{u}_g(t) \)):** Assume a given time history of ground acceleration from an earthquake.
- **Stiffness (k):** `k = m / T^2 = 1 / 1^2 = 1 N/m`.
- **Damping Coefficient (c):** `c = 2ζ√(km) = 2 × 0.05 × √(1 × 1) = 0.1 Ns/m`.

## Newmark-beta Method Parameters
- **Beta (β):** Assume `β = 0.25` (this is typical for average acceleration method, which is unconditionally stable).
- **Gamma (γ):** Assume `γ = 0.5`.

## Steps to Calculate the Spectral Acceleration

### 1. Discretize the Time History
- Assume the ground motion is given in a discrete form as \( \ddot{u}_g(t) \) at intervals of `Δt` (e.g., 0.02 seconds).

### 2. Initial Conditions
- Initial displacement `u₀ = 0` (typically zero for spectral analysis).
- Initial velocity `\(\dot{u}_0 = 0\)`.
- Initial acceleration `\(\ddot{u}_0 = \frac{-c \dot{u}_0 - k u_0 + m \ddot{u}_g(0)}{m}\)`.

### 3. Effective Stiffness and Force
Calculate the effective stiffness `\(k_{\text{eff}}\)` and effective force `\(F_{\text{eff}}^{n+1}\)` at each time step:

\[
k_{\text{eff}} = k + \frac{\gamma}{\beta \Delta t}c + \frac{1}{\beta \Delta t^2}m
\]

\[
F_{\text{eff}}^{n+1} = -m \ddot{u}_g^{n+1} + m\left(\frac{1}{\beta \Delta t^2} u^n + \frac{1}{\beta \Delta t} \dot{u}^n + \left(\frac{1}{2\beta} - 1\right)\ddot{u}^n\right) + c\left(\frac{\gamma}{\beta \Delta t} u^n + \left(\frac{\gamma}{\beta} - 1\right)\dot{u}^n + \Delta t \left(\frac{\gamma}{2\beta} - 1\right) \ddot{u}^n\right)
\]

### 4. Iterative Calculation
For each time step `\(t_{n+1}\)`:
- Calculate the displacement `\(u^{n+1}\)`:

\[
u^{n+1} = \frac{F_{\text{eff}}^{n+1}}{k_{\text{eff}}}
\]

- Calculate the velocity `\(\dot{u}^{n+1}\)`:

\[
\dot{u}^{n+1} = \gamma \frac{u^{n+1} - u^n}{\beta \Delta t} - \frac{\gamma - \beta}{\beta} \dot{u}^n + \Delta t \left(1 - \frac{\gamma}{2\beta}\right) \ddot{u}^n
\]

- Calculate the acceleration `\(\ddot{u}^{n+1}\)`:

\[
\ddot{u}^{n+1} = \frac{u^{n+1} - u^n}{\beta \Delta t^2} - \frac{\dot{u}^n}{\beta \Delta t} - \frac{1 - 2\beta}{2\beta} \ddot{u}^n
\]

### 5. Calculate the Absolute Acceleration
Calculate the absolute acceleration at each time step:

\[
\ddot{u}_{\text{abs}}^{n+1} = \ddot{u}^{n+1} + \ddot{u}_g^{n+1}
\]

### 6. Determine the Maximum Acceleration
The spectral acceleration `\(S_a\)` is the maximum absolute acceleration over all time steps:

\[
S_a = \max(\ddot{u}_{\text{abs}}^{n+1})
\]

## Example Calculation for One Time Step
Let's assume `\(\Delta t = 0.02\)` seconds and the following values for the first few steps:

- Ground acceleration: `\(\ddot{u}_g^0 = 0.1 m/s^2\)`, `\(\ddot{u}_g^1 = 0.2 m/s^2\)`, etc.

For the first time step:

- `\(\ddot{u}^0 = 0 m/s^2\)`
- `\(u^0 = 0 m\)`
- `\(\dot{u}^0 = 0 m/s\)`

Effective stiffness:

\[
k_{\text{eff}} = 1 + \frac{0.5}{0.25 \times 0.02} \times 0.1 + \frac{1}{0.25 \times 0.02^2} \times 1 = 1 + 10 + 2500 = 2511 N/m
\]

Effective force:

\[
F_{\text{eff}}^1 = -1 \times 0.2 + 1 \times \left(\frac{1}{0.25 \times 0.02^2} \times 0 + \frac{1}{0.25 \times 0.02} \times 0 + \left(\frac{1}{2 \times 0.25} - 1\right) \times 0 \right) + 0.1 \times \left(\frac{0.5}{0.25 \times 0.02} \times 0 + \left(\frac{0.5}{0.25} - 1\right) \times 0 + 0.02 \times \left(\frac{0.5}{2 \times 0.25} - 1\right) \times 0 \right)
\]

Simplified:

\[
F_{\text{eff}}^1 = -0.2 N
\]

Displacement `\(u^1\)`:

\[
u^1 = \frac{-0.2}{2511} = -7.96 \times 10^{-5} m
\]

Velocity `\(\dot{u}^1\)`:

\[
\dot{u}^1 = \gamma \frac{u^1 - u^0}{\beta \Delta t} = 0.5 \times \frac{-7.96 \times 10^{-5} - 0}{0.25 \times 0.02} = -7.96 \times 10^{-3} m/s
\]

Acceleration `\(\ddot{u}^1\)`:

\[
\ddot{u}^1 = \frac{u^1 - u^0}{\beta \Delta t^2} = \frac{-7.96 \times 10^{-5}}{0.25 \times 0.02^2} = -0.1592 m/s^2
\]

Absolute acceleration `\(\ddot{u}_{\text{abs}}^1\)`:

\[
\ddot{u}_{\text{abs}}^1 = \ddot{u}^1 + \ddot{u}_g^1 = -0.1592 + 0.2 = 0.0408 m/s^2
\]

## Final Result
You would repeat this process for each time step, and after processing all steps, you would take the maximum of all `\(\ddot{u}_{\text{abs}}\)` values as the spectral acceleration `\(S_a\)`.

This example demonstrates the basic steps for applying the Newmark-beta method to find the spectral acceleration. To fully automate this, you can implement the steps in a programming environment like MATLAB or Python.
