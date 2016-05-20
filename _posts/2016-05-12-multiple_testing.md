---
layout: post
title: "Basics of Model Selection: Likelihood Testing and Multiple Testing"
output: html_document
comments: true
---
  I am going to talk about model selection in statistical hypothesis perspective. A model can be defined by setting a likelihood. When two models are nested (one is a special case of the other one), the asymptotic distribution of -2 times log likelihood ratio converges to a chi-square distribution, so the statistical hypothesis can be constructed. Even though the likelihood ratio test is simple and strong tool for model selection, it can be only applied to nested models, and it is not appropriate to use it for the comparision of multiple models.
  
  Another popular testing-based approach for model selection is mutiple testing procedures. In linear models, by marginally considering each covariate, we can get $$p$$ number of t-statistics and its p-values, and we can select the variables whose p-value is small. Since Bonferroni's correction is too conservative, False Discovery Rate (FDR) control [(Benjamini and Hochberg, 1995)](http://www.math.tau.ac.il/~ybenja/MyPapers/benjamini_hochberg1995.pdf) is popular, which controls the expected proportion of Type I errors among the rejected hypotheses.
  
   However, these mutiple testing approaches have a critical problems when the tests statistics are correlated. Let me explain it with a simple example. Condier the model as 
   
   $$Y = X\beta_0 + \epsilon,$$   
   
   where $$\epsilon \sim N(0,0.1 I_n)$$ and the true regression coefficients $$\beta_0 = (1,1,-1,-1,-1,0,\dots,0)$$ with $$n=200$$ and $$p=20$$, So only first five variables are involved in the data-generating model. Also, suppose that the covariate is generated from the Gaussian distribution with a confound symmety covariance with correlation 0.5; i.e., $$\Sigma_{i,j}=0.5$$, if $$i\neq j$$, and $$0$$, otherwise for $$1\leq i,j\leq p$$. Similar settings with this are commonly used in many variable slection papers such as  [nonlocal prior paper](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3867525/) , [the reciprocal lasso](http://www.tandfonline.com/doi/abs/10.1080/01621459.2014.984812), and [the BASAD](https://arxiv.org/pdf/1405.6545.pdf). The below is the R code to generate the data. 

{% highlight r %}
n=200; p =20
Sig = matrix(0.5,p,p)
diag(Sig)=1
ch = chol(Sig)
X = matrix(rnorm(n*p),n,p)%*%ch
beta0 = rep(0,p)
beta0[1:5] = c(1,1,-1,-1,-1)
y = X%*%beta0 + rnorm(n)*sqrt(0.1)
{% endhighlight %}

{% highlight r %}
as.vector(round(cor(X,y),3))
{% endhighlight %}



{% highlight text %}
##  [1] -0.002 -0.036 -0.581 -0.638 -0.597 -0.325 -0.327 -0.350 -0.389 -0.311
## [11] -0.311 -0.344 -0.378 -0.382 -0.408 -0.287 -0.352 -0.329 -0.240 -0.304
{% endhighlight %}

  I calculated the absolute value of the marginal correlation between the response $$y$$ and each individual covariate $$X_j$$ for $$j=1,\dots,p$$. 
Intuitively, the first five correlation coefficients shoud be significantly larger than the others, since the true linear model includs the first five, but the first two correlation coefficients are almost zero. Since the t-statistic is a monotone transformation of the correlation coefficient, multiple testing procedures will never select the first two variables that are involved in the true model.

#### What's going on here?####

To see the details of the problem, let me write some covariance structure of the model. Roughly speaking, the correlation coefficients betweeen the response and the covariates can be identified by the following quantity

$$\begin{eqnarray*} 
Cor({\bf X},y) &\approx&  Cor\left( {\bf X} , - X_1 - X_2 + X_3 + X_4 + X_5 \right)\\
&=&  \begin{bmatrix}
1 & 0.5 & \dots & 0.5 \\
0.5 & 1 & \dots & 0.5 \\
0.5 & \dots& 1 &\dots \\
0.5& \dots  &  \dots & \dots\\
0.5 &\dots &\dots & 1\\
\end{bmatrix}
\begin{bmatrix}
-1\\
-1\\
1\\
1\\
1\\
0\\
\dots\\
0
\end{bmatrix}
\\
&=&
\begin{bmatrix}
0\\
0\\
1\\
1\\
1\\
0.5\\
\dots\\
0.5
\end{bmatrix}
 
\end{eqnarray*}
$$





