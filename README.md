# crisisnetwork

Systemic risk measures the risk of a breakdown in the financial markets, and provides the rationale for financial market regulation. It is distinct from systematic risk, which is aggregate risk that cannot be avoided through diversification. The measurement challenge lies not only in determining, at each moment, this potential for market breakdown, but also our uncertainty of said measure. Only then can we develop effective metrics for anticipating breakdowns before they occur. Bayesian state space models, which allow us to model uncertainty, are effective at quantifying this.

In this project, I model financial risk via a Bayesian latent threshold multivariate time-varying AR model. (<a href="http://ftp.stat.duke.edu/WorkingPapers/10-25.pdf">Nakajima & West, 2012</a>) Modeling this as a dynamic time-series is essential, since I am most concerned with how the markets evolve and change, especially during crisis time periods. Modeling this as a multivariate, rather than univariate, time series is also essential, since I wish to explore how panic spills from one instrument, one sector of the markets, to another, which involves examining how they are connected to each other. And when we have hundreds, if not thousands, of financial instruments to consider, it becomes essential to carry out some sort of dimension reduction.

One of the most common forms of this is PCA, where the top eigenvalues are identified and the rest discarded. However, in this project I use a different matrix rotation and another method of inducing sparsity. As described in Nakajima & West's paper, I create an lower-triangular matrix A(t) that captures the conditional dependencies between the different time series. There are several other layers to this model, including a parameter that induces sparsity by setting some of the terms in the A(t) and B(t) (where B(t) is the matrix of beta coefficients at time t) matrices to zero if they go below a certain threshold, the level of which is determined by the data.

This model also has applications outside the financial sector. As Nakajima and West explain in their paper, it presents a novel way to study dynamic networks. Furthermore, it can also be incorporated into a GLM (Poisson regression, logistic regression, etc) with a few tweaks.

<center><img src="https://github.com/kkamb/crisisnetwork/blob/master/alphasurface.png"></center><br>
<small>Figure 1: This is one of the outputs of this model, a graphical representation of the time-varying dependencies between different time series. The x-axis are the discovered parameters in the lower-triangular A(t) matrix, representing how the y-variables are dependent on each other, and the y-axis is the time (spanning 400 days).</small>

However, in its current form, it also has drawbacks. While it is very good at capturing gradual changes, it tends to miss dramatic ones. (I'm working on fixing this!) It is also extremely computationally taxing, and the parameters take a long time to converge. (I'm not sure how to fix this.)

To run the model, change the <a href="https://github.com/kkamb/crisisnetwork/blob/master/fcrisis.m">fcrisis.m</a> file to reflect your parameter settings and data source, and then run <a href="https://github.com/kkamb/crisisnetwork/blob/master/lt_mcmc2.m">lt_mcmc2.m</a>.
