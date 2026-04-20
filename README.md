# Equation-Automata
Without any mathematical functions, a cellular automaton outputs the graph of tan<sup>2</sup>(x) + tan(x)

The code requires PyCX Simulator to run, which can be downloaded from: https://github.com/hsayama/PyCX

## Derivation of Tangent Equation

In line 170 of equation.py, there is an inequality g / 8 > (1 - p) * p. g is the Moore neighbor count, and p is the initial probability of the system. If we assign 1 to g, and make this an equality, we get:

p² - p + 1 / 8 = 0

which has the roots:

p = (1 / 2) ± (1 / 2√ 2)

These are the probabilities that show first-order phase transitions. The first-order phase transitions and their immediate non-critical neighbors (upper for +, lower for -) draw the arms of the tangent graph.

The entire system evolves towards the inverse Ising 2-D critical temperature, which is shown in the magnetic susceptibility graph below as the Paramagnetic Response. That is the second-order phase transition.


![Ising model critical attractor magnetic susceptibility](https://github.com/user-attachments/assets/614f4272-0abd-4348-af1b-8ad1a012ab47)


To solve the tangent equation:

(1 / 2) ± (1 / 2√ 2) is cos²(π/8) and sin²(π/8) respectively.

cos²(π/8) - sin²(π/8) = cos(2*π/8) = cos(π/4) = sin(π/4)

cos²(π/8) - sin²(π/8) = 2sin(π/8)cos(π/8)

This equation is then generalized to:

acos²(x) - bsin²(x) = sin(x)cos(x)

divide both sides by cos²(x):

∴ btan²(x) + tan(x) - a = 0

which is exactly the equation at code line 208, and draws the tan²(x) + tan(x) graph.


<img width="566" height="918" alt="Tangent Function Output" src="https://github.com/user-attachments/assets/ab2fdbd2-cd67-420a-aa85-c0507270aaa5" />


![Wolfram Alpha Confirmation of Result](https://github.com/user-attachments/assets/9181d186-6d74-4817-9dc4-f3909a5f5a77)
