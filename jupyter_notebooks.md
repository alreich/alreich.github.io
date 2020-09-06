[[HOME](index.md)]

# Mathematical / Statistical Topics

The following provide links to Jupyter Notebooks that I've written on various topics in statistical analysis, machine learning, and data science.  There are also some links to related resources farther below (e.g., books on Bayesian data analysis).

## [Bayesian Beta-Binomial Example](https://nbviewer.jupyter.org/github/alreich/ipython-notebooks/blob/master/Bayesian_Beta_Binomial_Example.ipynb)

This notebook provides a very simple example of Bayesian parameter estimation using the Beta-Binomial model. Both analytical and simulation-based results are presented.  Three different approaches are used to obtain a parameter estimate for this model:

* Exact Analytical Solution
* Simple Non-MCMC Solution
* MCMC Solution

## [Covid-19 and Blood Types (using a Bayesian approach)](https://nbviewer.jupyter.org/github/alreich/ipython-notebooks/blob/master/covid19_and_blood_type.ipynb)

In the recent paper by Jiao Zhao, et al. it was reported that blood group (e.g., A, B, AB, or O) appears to have an effect on the likelihood of becoming infected with the Covid-19 virus. Basically, people with blood type A appear to be more susceptible to the virus, while people with blood type O appear to be less susceptible.

The authors of the paper performed several types of statistical analyses to arrive at their conclusion: one-way ANOVA, 2-tailed chi-square, and a meta-analysis using random effects models.  In this notebook, I've performed a different type of analysis, Bayesian Data Analysis (BDA), using the data reported in their paper.

[CAVEAT: No one has checked my work, so there could be errors in it] This BDA appears to support their conclusion, but also provides posterior density estimates for the proportions of A, B, AB, and O blood groups among the infected, along with credible intervals for those proportions. See the four posterior density plots at the end of this notebook.

## [Extreme Value Theory (EVT) Example](https://nbviewer.jupyter.org/github/alreich/ipython-notebooks/blob/master/EVT_Example.ipynb)

"EVT is widely used in many disciplines, such as structural engineering, finance, earth sciences, traffic prediction, and geological engineering. For example, EVT might be used in the field of hydrology to estimate the probability of an unusually large flooding event, such as the 100-year flood." -- [Wikipedia](https://en.wikipedia.org/wiki/Extreme_value_theory)

This Jupyter notebook describes EVT calculations, in Python, using an example from Stuart Cole's book, "An Introduction to Statistical Modeling of Extreme Values". It is noted that there does not appear to be a standard representation of the GEV distribution. Representations differ on how the shape parameter, ξ, should be expressed. Specifically, the shape parameter in the 'ismev' package in R is the negative of the shape parameter in the Python 'scipy.stats.genextreme' module.

## [Monoids 101 for Apache Spark](https://nbviewer.jupyter.org/github/alreich/ipython-notebooks/blob/master/Monoids_101_for_Apache_Spark.ipynb)

This notebook describes what monoids are and the role they play in reduction and aggregation in Spark, specifically PySpark. To illustrate the use of the monoid concept, the following examples are included:

* Word count
* Max/Min as monoids
* Histogram calculation using vectors as monoids
* Calculating sample means and standard deviations
* Calculating covariances and correlations using vectors and matrices as monoids
* Sets as monoids
* A HyperLogLog monoid (a "sketch method" for approximating set cardinality). NOTE: Uses the implementation, hll.py at https://github.com/Parsely/probably, which has been modified here to remove the dependency on the "smhasher" module and so that it can be run using the Anaconda Python distribution.

### Misc. Resources

* [Books & Python Libraries](bayes.md)
* [Bayesian Inference in R](https://cran.r-project.org/web/views/Bayesian.html)


[[HOME](index.md)]