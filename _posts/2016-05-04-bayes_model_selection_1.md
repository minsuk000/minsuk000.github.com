---
layout: post
title: "Bayesian Model Selection and Shrinkage Priors (1)"
tags: Bayesian model selection, shrinkage priors
comments: true
---
 There have been a lot of interest in model selection or variable selection methods.
  Because many data sets have"large p, small n" problem (or high-dimensional problem), it is crucial to reduce the dimension of the data, and the simplest way is to select a small number of  variables that well-fit to the model. I am going to introduce a simple introduction of this framework in Bayesian philosophy.
 
 You may be familar with linear regression models as follows:
 
 $$Y_i = \beta_0 + \beta_1 X_{1i} _ \beta_2 X_{2i} + \beta_3 X_{3i} + \epsilon_i$$

