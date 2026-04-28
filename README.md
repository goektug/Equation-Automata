# Equation Automata

This code contains no trigonometric functions. It imports numpy and matplotlib. It defines a grid, counts neighbors, and applies integer comparison rules. When you run it, it prints the graph of tan²(x) + tan(x). This is not a coincidence or a hidden import — it is a consequence of the geometry of the square lattice and the Ising criticality condition, derived below.
 
A 2D binary cellular automaton with no trigonometric functions recovers the exact critical temperature of the 2D Ising model and implements its stereographic projection onto the 1D linear Ising chain. The projection is mediated by the Gudermannian function:
 
**tan(π/8) = tanh(ln(1+√2)/2)**
 
This is Onsager's self-dual condition expressed as a single equation. The automaton outputs this identity numerically through simple neighborhood counting rules, with no mathematics beyond integer comparisons in the code.
 
---
 
## The Core Result
 
The automaton rule at line 170 is:
 
```python
if g / 8 > (1 - p) * p:
```
 
where `g` is the Moore neighbor count and `p` is the initial cell density. Setting `g = 1` and solving the equality:
 
```
p² - p + 1/8 = 0
p = 1/2 ± 1/(2√2)
```

At this point no trigonometry has appeared anywhere — only an integer neighbor count, a probability, and a quadratic equation. The trigonometry emerges from the double angle identity applied to the roots. 
The two roots are exactly:
 
```
cos²(π/8) = 1/2 + 1/(2√2) ≈ 0.8536
sin²(π/8) = 1/2 - 1/(2√2) ≈ 0.1464
```
 
These are the first-order phase boundaries of the system. Their difference encodes the Kramers-Wannier self-dual angle:
 
```
cos²(π/8) - sin²(π/8) = cos(π/4) = sin(π/4) = 1/√2
```
 
Since cos(π/4) = sin(π/4), this is simultaneously the double angle identity:
 
```
cos²(π/8) - sin²(π/8) = 2·sin(π/8)·cos(π/8)
```
 
General equation is: `a·cos²(x) - b·sin²(x) = sin(x)·cos(x)`, following above and substituting with a = b = 1/2 gives:
 
```
(1/2)·cos(2x) = (1/2)·sin(2x)
tan(2x) = 1
x = π/8
```
 
Dividing the generalized equation by cos²(x):
 
```
b·tan²(x) + tan(x) - a = 0
```
 
This is exactly the equation computed at line 208 of the code, which draws the tan²(x) + tan(x) graph.
 
---
 
## The Stereographic Projection
 
The automaton is a **stereographic projection of the 2D rectangular Ising model onto the 1D linear Ising chain**, mediated by the Gudermannian function.
 
The Kramers-Wannier duality condition for the 2D Ising model, expressed in Gudermannian form (following Onsager 1944), is:
 
```
gd(2H) + gd(2H*) = π/2
```
 
At the self-dual point H = H* = Kc = ln(1+√2)/2, this gives:
 
```
gd(2Kc) = π/4
```
 
The stereographic projection parameter is:
 
```
s = tan(φ/2) = tanh(ψ/2)
  where φ = π/4 (circular, self-dual angle)
  and   ψ = ln(1+√2) (hyperbolic, 2Kc)
```
 
Therefore:
 
```
s = tan(π/8) = tanh(Kc) = √2 - 1
```
 
This single value `s` is the automaton's fundamental output. It appears as `1/ratio1` in the print statement at line 208, computed entirely from the ratio of spatial statistics:
 
```
s = tanh(Kc) = (count0/count1) / (j/i)
```
 
where `count0/count1` is the dead-to-alive cell ratio (= sinh(Kc) in the hyperbolic picture) and `j/i` is the average Moore neighbor count per cell (= cosh(Kc)).
 
When sin(x) > 1 causes a range error in the circular picture, the Gudermannian provides the analytic continuation: sin(x) → sinh(ψ), cos(x) → cosh(ψ), tan(x) → tanh(ψ), with the map gd(ψ) = arcsin(tanh(ψ)) connecting the two regimes without complex numbers.
 
---
 
## Phase Diagram
 
The threshold integer in the Moore/von Neumann competition rule controls the depth of the Boltzmann self-consistency iteration:
 
| Threshold | Highest cell count | Nature |
|---|---|---|
| < 6 | 2/Kc ≈ 0.881 (inverse Ising critical temperature) | Critical — recovers 2D Ising |
| = 6 | σ(1/√2) ≈ 0.6697 | Ising critical temperature outputs same number of cells with state 1 = Second-order transition — geometric frustration |
| > 6 | Ising critical temperature input does not match the output | Non-critical — projection fails |
 
The threshold integer **6** is the last value sustaining criticality. It sits at the geometric boundary between Moore (8 neighbors) and von Neumann (4 neighbors) neighborhoods: 6/9 occupied cells force adjacency by the pigeonhole principle, magnetizing the neighborhood. This is the discrete stereographic pole — the point from which the 2D→1D projection is cast.
 
The two first-order phase transitions at sin²(π/8) ≈ 0.1464 and cos²(π/8) ≈ 0.8536 flank a second-order transition at σ(1/√2) ≈ 0.6697, giving a three-phase structure analogous to the Blume-Emery-Griffiths model.
 
---
 
## The Closed Cycle
 
At initialization p = 1/√2, the automaton traces a closed loop through the 1D Ising transfer matrix:
 
```
p = 1/√2
  → [automaton output x ≈ 0.3197]
  → tan²(x) + tan(x) = Kc
  → tanh(Kc) = √2 - 1 = tan(π/8)
  → (1 + tanh(Kc)) / 2 = 1/√2
```
 
The anchor identity closing this loop is:
 
```
tanh(Kc) = tan(π/8) = √2 - 1   [exact]
```
 
The 1D Ising conditional probability formula `Prob(σ₂=+1 | σ₁=+1) = (1 + tanh(βJ))/2` at βJ = Kc returns exactly 1/√2, completing the cycle. The automaton encodes both the 2D Ising critical temperature and the 1D transfer matrix fixed point in the same spatial statistics.
 
---
 
## Ising Model Correspondence
 
![Ising model critical attractor magnetic susceptibility](https://github.com/user-attachments/assets/614f4272-0abd-4348-af1b-8ad1a012ab47)
 
The critical attractor plot maps directly onto the magnetic susceptibility curves of the 2D Ising model:
 
- **Paramagnetic response** — the inverse Ising critical temperature Kc ≈ 0.4407 (second-order transition)
- **Ferrimagnetic response** — the upper fixed point cos²(π/8) ≈ 0.8536 (first-order boundary)
- **Antiferromagnetic response** — the lower fixed point sin²(π/8) ≈ 0.1464 (first-order boundary)
<img width="566" height="918" alt="Tangent Function Output" src="https://github.com/user-attachments/assets/ab2fdbd2-cd67-420a-aa85-c0507270aaa5" />
 
The critical and non-critical values correspond to the arms of the tan²(x) + tan(x) graph, confirmed by Wolfram Alpha:
 
![Wolfram Alpha Confirmation of Result](https://github.com/user-attachments/assets/9181d186-6d74-4817-9dc4-f3909a5f5a77)
 
---
 
## Running the Code
 
Requires PyCX Simulator:
 
```
https://github.com/hsayama/PyCX
```
 
Repository: [https://github.com/goektug/Equation-Automata](https://github.com/goektug/Equation-Automata)
 
---
 
## References
 
- Onsager, L. (1944). Crystal statistics I: A two-dimensional model with an order-disorder transition. *Physical Review*, 65(3-4), 117.
- Kramers, H. A., & Wannier, G. H. (1941). Statistics of the two-dimensional ferromagnet. *Physical Review*, 60(3), 252.
- Gudermannian stereographic projection: see Onsager (1944) Fig. 4 and F. Klein, reference therein pp. 293–299.
---
 
*Goktug Islamoglu, Freiburg, Germany*
*Contact: goktugislamoglu@gmail.com*




