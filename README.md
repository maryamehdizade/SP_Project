# 📈 Stochastic Processes: Gaussian Processes & Langevin Dynamics

![Python](https://img.shields.io/badge/python-3.8%2B-blue.svg)
![NumPy](https://img.shields.io/badge/numpy-latest-blue.svg)
![Matplotlib](https://img.shields.io/badge/matplotlib-latest-blue.svg)

This repository contains a Python-based Jupyter Notebook project exploring the fundamentals of Gaussian Processes (GPs), Bayesian hyperparameter inference, and Stochastic Differential Equations (SDEs) via Langevin Dynamics.

---

## 📑 Table of Contents
- [Project Overview](#-project-overview)
- [Dependencies](#-dependencies)
- [Tasks Overview](#-tasks-overview)
  - [Task 1: Gaussian Processes Prior Sampling](#task-1-gaussian-processes-prior-sampling)
  - [Task 2: Gaussian Process Regression](#task-2-gaussian-process-regression-posterior-prediction)
  - [Task 3: Hyperparameter Inference (MCMC)](#task-3-hyperparameter-inference-with-metropolis-hastings)
  - [Task 4: Langevin Dynamics (ULA)](#task-4-langevin-dynamics-unadjusted-langevin-algorithm---ula)

---

## 🔭 Project Overview

The project is divided into four main tasks:
1. Constructing and visualizing Gaussian Process prior spaces.
2. Implementing Gaussian Process regression for noisy observations.
3. Using Metropolis-Hastings MCMC to sample hyperparameters over the log-marginal likelihood surface.
4. Simulating an Itô diffusion using the Unadjusted Langevin Algorithm (ULA) to sample from a complex distribution.

## 🛠️ Dependencies

To run the notebook, you need the following libraries. You can install the required packages using `pip`:
```bash
pip install numpy matplotlib
```


## 🚀 Tasks Overview

### Task 1: Gaussian Processes Prior Sampling
> **🎯 Objective:** Visualize the "prior" assumptions made by different GP kernels before any data is observed.

**Explanation:**
To visualize the prior assumptions of different GPs, we implemented two distinct kernels: the Radial Basis Function (RBF) to model smooth, infinitely differentiable functions, and the Brownian motion kernel for rough, jagged paths. We established spatial domains for both models ($x \in [-5, 5]$ for RBF and $x \in [0, 10]$ for Brownian) and constructed their respective covariance matrices. A small jitter ($10^{-8}I$) was added to the diagonals to ensure numerical stability during matrix inversion. Finally, we drew and plotted five sample paths from the multivariate normal distribution $\mathcal{N}(0, K)$ to compare the structural "personalities" dictated by each kernel before any data observation.

### Task 2: Gaussian Process Regression (Posterior Prediction)
> **🎯 Objective:** Update the GP prior with noisy observed data to perform regression and quantify uncertainty.

**Explanation:**
To perform GP regression and quantify uncertainty, we first generated a synthetic dataset $\mathcal{D} = \{(x_i, y_i)\}_{i=1}^n$ using a sine function with added Gaussian noise. By applying the joint Gaussian partition, we derived the conditional posterior predictive distribution. This allowed us to analytically calculate the posterior mean $\mu_*$ and posterior covariance $\Sigma_*$ using the Linear Gaussian Conditioning Theorem. Using these updated parameters, we drew continuous function samples from the posterior. The final visualization highlights the noisy observations alongside the mean predictive line, a 95% confidence interval shaded region ($\mu_* \pm 2\sigma_*$), and individual posterior sample paths that are constrained to pass near the training data.

### Task 3: Hyperparameter Inference with Metropolis-Hastings
> **🎯 Objective:** Perform fully Bayesian inference on the RBF kernel's length scale ($\ell$) rather than using a fixed point estimate, balancing data fit and model complexity.

**Explanation:**
In task number 3 we try to find an optimal l such that by running the program 150,000 times we find an optimal l:
At each step we make a random move with an interval of 0.15 in the limited range l, if the result is better we change l to a new l, but if it is not better we check with a uniform distribution and it is possible to accept l if it is worse. It is possible to accept the worse to reach the optimal l, according to the given formula each l receives a score and l less than 0 is also acceptable to accept, finally we remove the first 20% for a better approximation and work with the final 12,000 data which are the result of our work.

### Task 4: Langevin Dynamics (Unadjusted Langevin Algorithm - ULA)
> **🎯 Objective:** Sample from an intractable or complex probability distribution (a bi-modal Gaussian mixture) by simulating a continuous-time Stochastic Differential Equation (SDE).

**Explanation:**
To sample from a complex bi-modal Gaussian mixture, we simulated a continuous-time Stochastic Differential Equation using the Unadjusted Langevin Algorithm (ULA). We formulated the Langevin diffusion process and defined an unnormalized bi-modal log-density function with peaks at $-2$ and $2$. Using central finite differences, we implemented a numerical score function $\nabla \log \pi(x)$ to calculate the gradients of the log-density. The simulation was driven by the Euler-Maruyama discrete time-stepping update, carefully tuned with a step size of $\epsilon = 0.02$ to balance mixing speed against discretization error. The final plots of the particle trajectory across the first 500 steps, along with the density histogram of the accumulated samples, confirm that the process successfully transitioned between modes and converged to the exact target stationary distribution.
