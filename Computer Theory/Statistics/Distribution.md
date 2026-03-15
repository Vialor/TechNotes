**Probability Distribution** is a line reflecting the probability of all events defined by RV  
For a RV X (discrete or not) and an event  
	P($X\in J$) = the area under the distribution line when $x\in J$﻿

For Discrete RV:
	**Probability Mass Function PMF**: $p_X(x)=P(X=x)=P(s\in S:X(s)\leq x)$﻿
	**Cumulative Distribution Function CDF**: $F_X(x)=P(X\leq x)=P(s\in S:X(s)=x)$﻿
For Continuous RV:
	**Probability Density Function PDF**: $f_X: P(x\in [a, b])=\int_a^b f_X(x)dx$

**Independent and Identically Distributed IDD**: RVs are independent from each other and share the same distribution
# Discrete Distributions
**Degenerate Distribution**
$$P_c(x) =
\begin{cases}
1, & \text{if } x = c \\
0, & \text{if } x \neq c
\end{cases}$$
**Bernoulli Distribution**: $Bernoulli(\theta)$
$$P_c(x) =
\begin{cases}
\theta, & \text{if } x = c \\
1-\theta, & \text{if } x \neq c
\end{cases}$$
**Binomial Distribution**: $Binomial(n,\theta)$
>Probability to happend x times out of n tries, each try has θ probability to succeed
>$Binomial(1,\theta)=Bernoulli(\theta)$

$$
P_X(x)=\binom{n}{x}\theta^x(1-\theta)^{n-x}
$$
**Poisson Distribution**: $Poisson(\lambda)$
>Probability to happen x times out of n(infinity) tries, λ is the expected frequency to happen within a period of time or a certain space
>$Poisson(\lambda) = \lim_{n \to \infty} \text{Binomial}(n, \frac{\lambda}{n})$

$$P_X(x)={{\lambda^y\over y!}e^{-\lambda}}$$
**Geometric Distribution**: $Geometric(\theta)$
> Probability to happen only at the x-th try, each try has θ probability to succeed

$$
P_X(x)=(1-\theta)^{x-1}\theta
$$
**Negative Binomial Distribution**: $NegBinomial(\gamma,\theta)$
>$NegBinomial(1,\theta)=Geometric(\theta)$


# Continuous Distributions
**Continuous Uniform Distribution**: $Uniform(a,b)$
$$f(x) =
\begin{cases}
1\over b-a, & \text{if } a\leq x\leq b \\
0, & \text{otherwise}
\end{cases}$$
**Normal/Gaussian Distribution**: $Normal(\mu,\sigma)$
>μ: mean; σ: standard deviation
>Standard Normal Distribution: $Normal(0,1)$

$$f(x) =
{1\over \sqrt{2\pi}\sigma}e^{-{1\over2}({x-\mu\over\sigma})^2}$$
$P(\mu-\sigma\leq x\leq \mu+\sigma) \approx 68\%$
$P(\mu-2\sigma\leq x\leq \mu+2\sigma) \approx 95\%$
