
# MTH 210 Course Project: Statistical Computing

This repository contains the R implementation for estimating the parameters of a specific probability density function (PDF) using Maximum Likelihood Estimation (MLE). The project also constructs 95% confidence intervals for these parameters using both non-parametric and parametric bootstrap methods.

**Author:** Piyush Verma
**Roll Number:** 240749
**Institution:** Indian Institute of Technology Kanpur
**Course:** MTH 210 (Statistical Computing)

## Overview

The objective of this project is to model a dataset (`fort.58`) generated from a distribution where the probability density function for a random variable $X$ is given by:

$$f(x;\alpha,\lambda)=\alpha\lambda e^{-\lambda x}(1-e^{-\lambda x})^{\alpha-1}$$

for $x>0$ and $\alpha,\lambda>0$.

Since the log-likelihood function forms a two-dimensional optimization problem, it is reduced to a one-dimensional problem by profiling out the parameter $\alpha$. A Newton-Raphson algorithm is then utilized to find the roots of the first derivative of the profile log-likelihood, $g(\lambda)=0$.

## Features and Methodology

### 1. Newton-Raphson Optimization

* **Step-Halving:** The algorithm updates estimates iteratively but uses a step-halving technique ($\lambda_{k+1}=\lambda_{k}/2$) to prevent the unconstrained step from proposing a negative, invalid value for $\lambda$.


* **Initialization:** The starting estimate is initialized as the inverse of the sample mean ($\lambda_{0}=\frac{1}{\overline{x}}$).


* **Stopping Criteria:** The loop terminates based on either a derivative tolerance ($|g(\lambda_{k})|<10^{-12}$) or a step-size tolerance ($|\lambda_{k+1}-\lambda_{k}|<10^{-6}$). The loop is capped at 1000 iterations to prevent infinite looping.



### 2. Confidence Intervals (Bootstrap Methods)

The project runs 1000 iterations for both bootstrap methods to construct the 95% confidence intervals.

* **Non-Parametric Bootstrap:** Generates resamples of the dataset with replacement and applies the Newton-Raphson routine to each new sample.


* **Parametric Bootstrap:** Assumes the data follows the specified PDF and leverages the Probability Integral Transform ($Y=1-e^{-\lambda X}$) where $Y\sim Beta(\alpha,1)$ to generate simulated data from the distribution, which is then inverted and optimized.



## Code Structure

The implementation is contained within a single R script that executes the following workflow:

* **Dataset Initialization:** The `fort.58` dataset is embedded directly as a vector `df`.


* **Core Functions:** Features `d_log_likelihood` and `double_d_log_likelihood` to calculate the exact first and second derivatives analytically.


* **Optimization Routine:** The `NR` function executes the Newton-Raphson algorithm.


* **Bootstrapping:** Two distinct loops run the parametric and non-parametric bootstrap resampling, utilizing a set seed (`2026`) for complete reproducibility.


* **Visualization:** Plots the profile log-likelihood curve against $\lambda$ to visually verify the computed MLE, marking the exact maximum with a red point and a dashed vertical line.



## Results

The Newton-Raphson algorithm successfully converged to the global maximum in 5 iterations.

**Maximum Likelihood Estimates:**

* $\lambda_{MLE} = 2.603779$

* $\alpha_{MLE} = 2.67826$


**95% Confidence Intervals:**

| Method | 95% CI for $\lambda$ | 95% CI for $\alpha$ |
| --- | --- | --- |
| **Non-Parametric Bootstrap** | (2.121391, 3.674640) | (1.733191, 6.398400)|
| **Parametric Bootstrap** | (1.836909, 3.969332)| (1.703052, 5.958413)|

## Installation and Usage

To run this project locally, follow these steps:

**Clone the Repository**  
Open your terminal or command prompt and run the following command to download the project:

```bash
git clone [https://github.com/PIYUSH06VERMA/Numerical-Optimization-and-Paramteric-Estimation.git](https://github.com/PIYUSH06VERMA/Numerical-Optimization-and-Paramteric-Estimation.git)
```
* Environment Setup
  
Ensure you have R installed on your system.

Using an IDE like RStudio is highly recommended for viewing the profile log-likelihood plots seamlessly.

* Execution
  
Navigate to the cloned repository folder.

Open the 240749_Piyush_Verma.R script in your R environment.

Run the entire script. The script is completely self-contained and embeds the fort.58 dataset directly within the code, requiring no external file loading.

Upon execution, the console will output the Newton-Raphson iteration details, the Maximum Likelihood Estimates (MLEs), and the 95% confidence intervals. It will also automatically generate the profile log-likelihood plot in your plot viewer.
