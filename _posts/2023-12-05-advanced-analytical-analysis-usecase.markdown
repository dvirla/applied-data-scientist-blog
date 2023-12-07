---
layout: post
title: Advanced Statistical Analysis to Explore Cardiovascular Data 
date: 2023-12-05 00:00:00 +0300
description: How to apply an advanced statistical analysis to explore a cardiovascular dataset, gain insights and answer research questions. # Add post description (optional)
img: statistical-analysis.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Applied Data Science, Statistical Analysis, Data Analysis, Advanced Statistics]
---
# Exploring Framingham Heart Disease Data: An Advanced Statistical Analysis

As an applied data scientist, I recently had the opportunity to work on a fascinating project involving the Framingham Heart Study dataset. This dataset, renowned for its extensive data on cardiovascular health, allowed me to delve into advanced statistical analyses, unveiling insights into various factors influencing heart disease.

## Project Overview

### Dataset
The Framingham Heart Study dataset is a goldmine of information, covering a wide array of variables related to cardiovascular health. In this project, I aimed to answer several research questions using advanced statistical methods.

### Research Questions

In this project I aimed to answer several research questions, some of them are:

1. **Does Glucose Influence BMI?**
2. **Does Cholesterol Influence the Chance of Experiencing Congenital Heart Defects?**
3. **Does BMI Distribution Change Between Genders?**
4. **What Are the Different Relationships Between Heart Rate, Number of Cigarettes Per Day, and BMI Over Total Cholesterol?**

<div style="text-align:center;">
  <img src="../assets/img/bmi-distribution.png" alt="BMI Distributions" width="600"/>
  <p style="color:grey;"><a href="https://github.com/dvirla/Advanced-Statistical-Analysis/blob/main/Confidence%20Intervals%20%26%20Statistical%20Tests.ipynb">BMI Distribution Per Gender</a></p>
</div>

## Methodology

To answer these questions, I employed a variety of advanced statistical methods, including different regression models, handling missing data through imputations, conducting statistical tests, and leveraging Bayesian inference. The analysis was organized into distinct Jupyter notebooks within the GitHub repository for transparency and reproducibility.

<div style="text-align:center;">
  <img src="../assets/img/residual-analysis.png" alt="Residual Analysis" width="600"/>
  <p style="color:grey;"><a href="https://github.com/dvirla/Advanced-Statistical-Analysis/blob/main/Effects%20of%20Different%20Variables%20Over%20SysBP.ipynb">Residual Analysis of a Regression Model</a></p>
</div>

## Repository Structure

The project repository, [Advanced-Statistical-Analysis](https://github.com/dvirla/Advanced-Statistical-Analysis), is organized into several Jupyter notebooks, each focusing on a specific aspect of the analysis:

1. [Bayesian Inference & Handling Missing Data.ipynb](https://github.com/dvirla/Advanced-Statistical-Analysis/blob/main/Bayesian%20Inference%20%26%20Handling%20Missing%20Data.ipynb)
2. [Confidence Intervals & Statistical Tests.ipynb](https://github.com/dvirla/Advanced-Statistical-Analysis/blob/main/Confidence%20Intervals%20%26%20Statistical%20Tests.ipynb)
3. [EDA & Raising Hypotheses.ipynb](https://github.com/dvirla/Advanced-Statistical-Analysis/blob/main/EDA%20%26%20Raising%20Hypotheses.ipynb)
4. [Effects of Different Variables Over SysBP.ipynb](https://github.com/dvirla/Advanced-Statistical-Analysis/blob/main/Effects%20of%20Different%20Variables%20Over%20SysBP.ipynb)

These notebooks cover the entire spectrum of the analysis, from exploratory data analysis and hypothesis generation to Bayesian inference and handling missing data.

## Conclusion

The Framingham Heart Disease project provided valuable insights into the intricate relationships within cardiovascular health. By employing advanced statistical methods, I was able to navigate the complexities of the dataset and draw meaningful conclusions. The project's transparency and reproducibility are ensured through the structured organization of Jupyter notebooks within the GitHub repository.

Feel free to explore the [repository](https://github.com/dvirla/Advanced-Statistical-Analysis) to gain a deeper understanding of the analysis and methodologies employed. If you have any questions or suggestions, don't hesitate to reach out. Happy coding!