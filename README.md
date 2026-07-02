# FlamApp R&D AI Assignment

This repository contains my solution for the AI R&D Assignment to find the unknown variables ($\theta$, $M$, $X$) for the given parametric curve.

## Approach and Thought Process

Here is the step-by-step process I followed, which you can also see in the `notebook/assigmnet.ipynb` file.

### 1. Exploratory Data Analysis (EDA)
First, I loaded the `xy_data.csv` to see what the data looks like. I checked the shape and plotted a simple scatter plot. It looked like a wavy curve. 

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

### Desmos Format
As required in the PDF, here is the final equation in LaTeX format that can be copied directly to the Desmos calculator:

```latex
\left(t*\cos(0.5236)-e^{0.03\left|t\right|}\cdot\sin(0.3t)\sin(0.5236)+55, 42+t*\sin(0.5236)+e^{0.03\left|t\right|}\cdot\sin(0.3t)\cos(0.5236)\right)
```

Thank you for reviewing my assignment!
