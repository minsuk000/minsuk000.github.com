---
layout: post
title: "Basics of Model Selection: Bayes Factor and Posterior Model Probability"
output: html_document
comments: true
---


In the previous posts, I explained some fundamental concepts about model selection, and now I am going to talk about Bayesian model selection.  

The beauty of Bayesian statistIcs is a coherence.  All methodologies can be explained by one single theorem: Bayes theorem.  This also can be applied to model selection or hypothesis testing problems .  Let me first start with a simple example. Consider that the data $$y_i$$ follows i.i.d $$N(0,1)$$ for $$i=1,\dots,n$$, and suppose that we are interested in whether the parameter $$\theta$$ is zero or not. The natural hypothesis statement is

$$
M_0: \: \theta = 0 \:\:vs\:\:M_1: \: \theta \neq 0  
$$

The reason why I used $$M_0$$ and $$M_1$$, instead of more common notations $$H_0$$ and $$H_1$$ is that this can be viewed as a model selection problem; If $$\theta = 0$$, the model is going to be $$y_i \overset{i.i.d.}{\sim} N(0,1)$$ without any parameter, or the model is $$y_i \overset{i.i.d.}{\sim} N(theta,1)$$ with one parameter $$\theta$$, for $$i=1,\dots,n$$.  

### How can we compare two models $$M_0$$ and $$M_1$$? 

As usually Bayesian methods do, the Bayes theorem can be applied to the model selection problem. Suppose that we consider a Gaussian prior $$N(0,1)$$ on the parameter $$\theta$$ under $$M_1$$. Then, the posterior probability of each model can be expressed as

$$\begin{eqnarray*}
\pi(M_0\mid y) &\propto& p(y\mid \theta = 0)\pi(M_0)\\
\pi(M_1\mid y) & = & \int p(M_1, \theta\mid y )  d\theta \\
&\propto& \int p( y  \mid  M_1, \theta )\pi(\theta\mid  M_1) \pi(M_1)  d\theta = m_1(y)\pi(y),
\end{eqnarray*}
$$  

where $$\pi(\theta\mid M_1)$$ is the prior on $$\theta$$ and $$pi(M_k)$$ is the prior on the models, and $$m_k(y)$$ is the marginal likelihood of $$M_k$$ for $$k=0, 1$$.    

Then, a problem arises. We have to choose the prior on $$\theta$$. Which prior do we use? Even if we choose the prior, we have to choose its hyperparameter, and in many cases, the resulting posterior inference is sensitive to the choice of the hyperparameter. To simplify the problem, consider a Gaussian prior with a zero mean and a $$\tau^2$$ variance on $$\theta$$; $$\theta \sim N(0,\tau^2)$$. The resulting posterior probability of $$M_0$$ is 

$$\begin{eqnarray*}
pi(M_0\mid y) &=&  \frac{m_0(y)\pi(M_0)}{m_0(y)\pi(M_0)+m_1(y)\pi(M_1)}\\
&=&\frac{1}{1+BF_{10}(y)},
\end{eqnarray*}
$$ 

where $$BF_{10}(y)= m_1(y)\pi(M_1)/\{m_0(y)\pi(M_0)\}$$ is the Bayes factor of the hypothesis testing. The Bayes factor has a closed form as

$$
BF_{10}(y) = \tau^{-1}(n+1/\tau^2)^{-1/2}\exp\{ (n+1/\tau^2)\widetilde y^2/(2\sigma^2) \}\frac{\pi(M_1)}{\pi(M_0)},
$$

where $$\widetilde y = \sum_{i=1}^n y_i/(n+1/\tau^2)$$. Note that the smaller $$BF_{10}(y)$$, the more evidence in favor of $$M_0$$. 

Since $$(n+1/\tau^2)\widetilde y^2/(2\sigma^2)=O_p(1)$$, when the data is generated from $$M_0$$, the evidence in favor of $$M_0$$ ($$1/BF_{10}(y)$$) is determined by i) the value of $$\tau^2$$, and ii) the model prior ratio $$\frac{\pi(M_1)}{\pi(M_0)}$$. Surprisingly, as $$\tau^2$$ increases, the resulting evidence more prefers $$M_0$$, because of the $$\tau^{-1}$$ term. When $$\tau^2$$ is very large, the prior would have sparse density around the null value $$0$$, which results in a nonlocal-like prior ([Johson and Rossel, 2010](https://www.stat.tamu.edu/~vjohnson/files/JRSSB.72.2.2010.143-170.pdf)). Actually, that is the reason why nonlocal priors provide a model selection that has a stronger sparsity compared to that from Gaussian priors.  

Under Gaussian sequence models or linear regression models, there has been deep investigations in theories regarding model selection problems such as [Castillo and van der vaart. (2012)](https://projecteuclid.org/euclid.aos/1351602537) and  [Bontemps (2011)](https://arxiv.org/pdf/1009.1370.pdf) in situations where the number of parameter increases as the sample size increases. Even though the parameter is multiple in high-dimensional situations, the Bayesian model selection procedure is similar with the above example. The key is to control multiplicity problems, and it can be handled by  i) priors on the parameters, ii) model priors, or iii) both.  

Some people might be curious about the fact that the very flat Gaussian prior with large $$\tau^2$$ results in a strong evidence in favor of sparsity  regarding $$\theta$$, as the shape of the prior is exactly opposite of that of the [Baysian lasso](http://www.stat.ufl.edu/~casella/Papers/Lasso.pdf). One remark is that Bayesian lasso is not for Bayesian hypothesis tests or model selections, but for an estimation perspective. Strong shrinkage effects of the Bayesian lasso result from more spiky shape at the null value, but the Bayesian lasso prior (double-exponential priors) does not results in a strong evidence in favor of sparsity when it is used for Bayesian hypothesis tests or model selections. Sounds controversial, huh?