# FlamApp R&D AI Assignment

This repository contains my solution for the AI R&D Assignment to find the unknown variables ($\theta$, $M$, $X$) for the given parametric curve.

## Approach and Thought Process

Here is the step-by-step process I followed, which you can also see in the `assigmnet.ipynb` file.

### 1. Exploratory Data Analysis (EDA)
First, I loaded the `xy_data.csv` file and performed some basic exploratory data analysis to understand the dataset. I checked its dimensions, data types, summary statistics, and missing values. Finally, I plotted the points using a scatter plot to observe the overall shape of the data. The plot showed a smooth, continuous curve, suggesting that the points were generated from an underlying parametric equation.
### 2. Initial Attempt (Point-by-Point)
At first, I thought that the rows in the CSV file were in the exact order of $t$ from 6 to 60. So I made a simple loss function that compares each data point to the equation by assuming $t$ increases linearly.
I tried using simple optimizers like **Random Search** and **Nelder-Mead** on this. But they all got stuck at very high errors and gave almost the same bad loss. This made me realize that the optimization algorithm was not the main problem, but my assumption about $t$ was wrong. 

### 3. Investigation & Observation
I realized that the points in the dataset might not be in the exact order of $t$. When looking closely at the given parametric equations, I noticed they look exactly like a 2D rotation matrix formula. 
It means the curve is basically a function that is rotated by an angle $\theta$ and shifted by $X$.

### 4. Mathematical Insight
Because of this observation, I changed the math. Instead of guessing $t$, we can reverse the rotation to find the exact $t$ for any given point $(x, y)$ if we have a guess for $\theta$ and $X$. 
Once we calculate $t$, we can plug it back into the equations to find the predicted $x$ and $y$, and then measure the error (distance) from the actual points.

### 5. Improved Optimization
With this new and much better loss function, I used **Differential Evolution** because it is good at finding the global minimum without getting stuck in local minimums. After it found a very good result, I used **Nelder-Mead** for a final local refinement to get the most accurate numbers. Finally, I plotted the residual error to confirm the fit is really good.

## Final Results

The optimization found the following values for the unknowns:
- $\theta = 30^\circ$ (which is $0.5236$ radians)
- $M = 0.03$
- $X = 55$

As required by the assignment rubric, I also explicitly calculated the **L1 distance** between the actual data points and the final predicted curve points. The final L1 error (both mean and total) is printed at the end of the notebook to prove the accuracy of the result!

### Desmos Format
As required in the PDF, here is the final equation in LaTeX format that can be copied directly to the Desmos calculator:

```latex
\left(t*\cos(0.5236)-e^{0.03\left|t\right|}\cdot\sin(0.3t)\sin(0.5236)+55, 42+t*\sin(0.5236)+e^{0.03\left|t\right|}\cdot\sin(0.3t)\cos(0.5236)\right)
```

Thank you for reviewing my assignment!

## Citations / References

- **SciPy Optimization Library** (used for Differential Evolution and Nelder-Mead):
  Virtanen, P., Gommers, R., Oliphant, T. E., Haberland, M., Reddy, T., Cournapeau, D., ... & SciPy 1.0 Contributors. (2020). SciPy 1.0: fundamental algorithms for scientific computing in Python. *Nature methods*, 17(3), 261-272.
- **Rotation Matrices** (insight used to reformulate the objective function):
  Weisstein, Eric W. "Rotation Matrix." From MathWorld--A Wolfram Web Resource. https://mathworld.wolfram.com/RotationMatrix.html
- **Pandas Library** (used for Data Loading & EDA):
  McKinney, W. (2010). Data structures for statistical computing in python. In *Proceedings of the 9th Python in Science Conference* (Vol. 445, pp. 51-56).
