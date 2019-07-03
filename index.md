---
layout: default
title: Al Reich
tagline: whatev
description: blah, blah, blah
---

# Brief Bio

I’m a retired mathematician. My area of specialty is Mathematical Statistics. I worked in industry for my entire career in the area of scientific computing, including:
* Spacecraft Navigation 1980-87 (e.g., Space Shuttle onboard & ground navigation)
* Artificial Intelligence 1987-1995 (e.g., Automated Planning, Temporal Reasoning)
* Semiconductor Manufacturing 1995-2007 (e.g., Optical Proximity Correction, Design for Manufacturing)
* National Defense (2007-2017) (e.g., Data Science, Machine Learning, Cyber Security)

Most of my work has involved computer programming that, over my decades-long career, has involved about a dozen different programming languages.

## My Publications & Patents

A list of my papers and patents, sorted by number of citations, can be found at [Google Scholar](https://scholar.google.com/citations?user=N_wnSyUAAAAJ&hl=en).

## My Photography Hobby

I'm an avid photographer. Many of my photos can be seen at [Flickr](https://www.flickr.com/photos/alreich).

# Mathematical / Statistical Topics

The following provide links to Jupyter notebooks on various topics in statistical analysis, machine learning, and data science:

## Extreme Value Theory (EVT)

[This Jupyter notebook describes EVT calculations](https://github.com/alreich/ipython-notebooks/blob/master/EVT_Example.ipynb) using an example from Stuart Cole's book, "An Introduction to Statistical Modeling of Extreme Values". The calculations are done using both Python and R. It is noted that there does not appear to be a standard representation of the GEV distribution. Representations differ on how the shape parameter, ξ, should be expressed. Specifically, the shape parameter in the 'ismev' package in R is the negative of the shape parameter in the Python 'scipy.stats.genextreme' module.

## Monoids 101 for Apache Spark

[This notebook describes what monoids are and the role they play in reduction and aggregation in Spark, specifically PySpark](https://github.com/alreich/ipython-notebooks/blob/master/Monoids_101_for_Apache_Spark.ipynb). To illustrate the use of the monoid concept, the following examples are included:

* Word count
* Max/Min as monoids
* Histogram calculation using vectors as monoids
* Calculating sample means and standard deviations
* Calculating covariances and correlations using vectors and matrices as monoids
* Sets as monoids
* A HyperLogLog monoid (a "sketch method" for approximating set cardinality). NOTE: Uses the implementation, hll.py at https://github.com/Parsely/probably, which has been modified here to remove the dependency on the "smhasher" module and so that it can be run using the Anaconda Python distribution.

## Quantile Example using R-8 Method

Someone asked me once how to compute [quantiles](https://en.wikipedia.org/wiki/Quantile). [This notebook represents a partial answer to that question](https://github.com/alreich/ipython-notebooks/blob/master/Quantile_Example.ipynb). It provides examples of quantiles for a specific random sample using two versions of the R-8 method: (1) the ['mquantile' function](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mstats.mquantiles.html) found in the Python 'scipy.stats.mstats' module, and (2) a simple Python implementation based on [Wikipedia's description of the R-8 method](https://en.wikipedia.org/wiki/Quantile#Estimating_quantiles_from_a_sample).

## Bayesian Data Analysis

* [Resources](bayes.md)
