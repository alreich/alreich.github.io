[[HOME](index.md)]

# Mathematical / Statistical Topics

The following provide links to tutorials that I've written on various topics in statistical analysis, machine learning, and data science.  There are also some links to related resources farther below (e.g., books on Bayesian data analysis).

*NOTE: In case it helps, I run the Jupyter notebooks from Anaconda-Navigator using different environments for each notebook.*

## [Sqrt(-1) Explained](https://github.com/alreich/ipython-notebooks/blob/master/sqrt_minus_one_explained.ipynb)

This notebook is my attempt to provide a very very succinct explanation of the square root of -1.

## [Extreme Value Analysis (EVA)](https://nbviewer.org/github/alreich/EVA_talk/blob/main/Intro_to_EVA.pdf)

"EVA is widely used in many disciplines, such as structural engineering, finance, earth sciences, traffic prediction, and geological engineering. For example, EVA might be used in the field of hydrology to estimate the probability of an unusually large flooding event, such as the 100-year flood." -- [Wikipedia](https://en.wikipedia.org/wiki/Extreme_value_theory)

This is the PDF file from a presentation on Extreme Value Analysis (EVA) that I gave to the central Texas IEEE Section on Oct 20, 2022.

## [Covid-19 and Blood Types (using a Bayesian approach)](https://nbviewer.jupyter.org/github/alreich/ipython-notebooks/blob/master/covid19_and_blood_type.ipynb)

In the recent paper by Jiao Zhao, et al. it was reported that blood group (e.g., A, B, AB, or O) appears to have an effect on the likelihood of becoming infected with the Covid-19 virus. Basically, people with blood type A appear to be more susceptible to the virus, while people with blood type O appear to be less susceptible.

The authors of the paper performed several types of statistical analyses to arrive at their conclusion: one-way ANOVA, 2-tailed chi-square, and a meta-analysis using random effects models.  In this Jupyter notebook, I've performed a different type of analysis, Bayesian Data Analysis (BDA), using the data reported in their paper.

[CAVEAT: No one has checked my work, so there could be errors in it] This BDA appears to support their conclusion, but also provides posterior density estimates for the proportions of A, B, AB, and O blood groups among the infected, along with credible intervals for those proportions. See the four posterior density plots at the end of this notebook.

## [Bayesian Beta-Binomial Example](https://nbviewer.jupyter.org/github/alreich/ipython-notebooks/blob/master/Bayesian_Beta_Binomial_Example.ipynb)

This Jupyter notebook provides a very simple example of Bayesian parameter estimation using the Beta-Binomial model. Both analytical and simulation-based results are presented.  Three different approaches are used to obtain a parameter estimate for this model:

* Exact Analytical Solution
* Simple Non-MCMC Solution
* MCMC Solution

## [Monoids 101 for Apache Spark](https://nbviewer.jupyter.org/github/alreich/ipython-notebooks/blob/master/Monoids_101_for_Apache_Spark.ipynb)

This Jupyter notebook describes what monoids are and the role they play in reduction and aggregation in Spark, specifically PySpark. To illustrate the use of the monoid concept, the following examples are included:

* Word count
* Max/Min as monoids
* Histogram calculation using vectors as monoids
* Calculating sample means and standard deviations
* Calculating covariances and correlations using vectors and matrices as monoids
* Sets as monoids
* A HyperLogLog monoid (a "sketch method" for approximating set cardinality). NOTE: Uses the implementation, hll.py at https://github.com/Parsely/probably, which has been modified here to remove the dependency on the "smhasher" module and so that it can be run using the Anaconda Python distribution.

### Misc. Resources

* [Books & Python Libraries](bayes.md)
* "A modeler's guide to extreme value software", [arXiv:2205.07714v1](https://arxiv.org/abs/2205.07714), Léo R. Belzile, et al., 16 May 2022
* [Bayesian Inference in R](https://cran.r-project.org/web/views/Bayesian.html)


[[HOME](index.md)]
