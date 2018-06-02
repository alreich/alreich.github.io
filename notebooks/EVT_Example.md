
# Extreme Value Statistics - An Example (v2)

The example here is from Stuart Cole's book, <i>"An Introduction to Statistical Modeling of Extreme Values"</i>.

<p>The same example is also featured in the R package, <i><b>ismev</b></i>, (see http://cran.r-project.org/web/packages/ismev/).


```
import numpy as np
np.__version__
```




    '1.8.0'




```
import matplotlib as plt
print plt.__version__
```

    1.3.1



```
import pandas as pd
print pd.__version__
```

    0.12.0



```
import scipy as sp
from scipy.optimize import minimize
from scipy.stats import genextreme
sp.__version__
```




    '0.13.2'



## DATA: Annual maximum sea-levels at Port Pirie, South Australia

<b>Note on modules used here:</b> Pandas is used to read the CSV file into a Pandas DataFrame, but only for convenience.  Later, when the data analysis starts, the max sea level data will be turned into a standard NumPy ndarray.


```
def parse(yr):
    return datetime.datetime(yr,1,1)

df = pd.read_csv('port_pirie.csv', parse_dates=True, date_parser=parse, index_col='Year')
df.describe()
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SeaLevel</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td> 65.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>  3.980615</td>
    </tr>
    <tr>
      <th>std</th>
      <td>  0.240513</td>
    </tr>
    <tr>
      <th>min</th>
      <td>  3.570000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>  3.830000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>  3.960000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>  4.110000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>  4.690000</td>
    </tr>
  </tbody>
</table>
</div>




```
df.head()
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SeaLevel</th>
    </tr>
    <tr>
      <th>Year</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1923-01-01</th>
      <td> 4.03</td>
    </tr>
    <tr>
      <th>1924-01-01</th>
      <td> 3.83</td>
    </tr>
    <tr>
      <th>1925-01-01</th>
      <td> 3.65</td>
    </tr>
    <tr>
      <th>1926-01-01</th>
      <td> 3.88</td>
    </tr>
    <tr>
      <th>1927-01-01</th>
      <td> 4.01</td>
    </tr>
  </tbody>
</table>
</div>




```
df.tail()
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SeaLevel</th>
    </tr>
    <tr>
      <th>Year</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1983-01-01</th>
      <td> 4.08</td>
    </tr>
    <tr>
      <th>1984-01-01</th>
      <td> 3.90</td>
    </tr>
    <tr>
      <th>1985-01-01</th>
      <td> 3.88</td>
    </tr>
    <tr>
      <th>1986-01-01</th>
      <td> 3.94</td>
    </tr>
    <tr>
      <th>1987-01-01</th>
      <td> 4.33</td>
    </tr>
  </tbody>
</table>
</div>




```
figure(figsize=(12,6))
title('Port Pirie Annual Maximum Sea Level')
xlabel('Year')
ylabel('Meters')
x = [idx.year for idx in df.index]
y = df.values
plot(x,y)
```




    [<matplotlib.lines.Line2D at 0x10a816210>]




![png](EVT_Example_files/EVT_Example_11_1.png)


### Histogram of Data


```
max_sea_levels = df['SeaLevel']
```


```
figure(figsize=(12,6))
title("Port Pirie Annual Maximum Sea Level Histogram")
xlabel("Meters")
ylabel("Count per Bin")
result = hist(max_sea_levels, bins=arange(3.0,5.0,0.2), color='Yellow')
print "Bin Boundaries: %s" % result[1]
print "    Bin Counts: %s" % result[0]
```

    Bin Boundaries: [ 3.   3.2  3.4  3.6  3.8  4.   4.2  4.4  4.6  4.8]
        Bin Counts: [  0.   0.   1.  15.  23.  13.  10.   2.   1.]



![png](EVT_Example_files/EVT_Example_14_1.png)


## The Generalized Extreme Value (GEV) Distribution

The GEV probability density is given by: $G(z;\mu,\sigma,\xi) = \exp \left( - \left[ 1 + \xi \left( \frac {z - \mu} {\sigma} \right) \right]^{-1/\xi} \right)$ where $-\infty < \mu < \infty$, $\sigma > 0$, $-\infty < \xi < \infty$, and $z$ is such that $1 + \xi(z - \mu)/\sigma > 0$

<b><u>IMPORTANT NOTE</u>:</b> There does not appear to be a standard representation of the GEV distribution.  Algorithms appear to differ on how the <i><b>shape</b></i> parameter, $\xi$, should be represented.  Specifically, the shape parameter in the <i><b>ismev</b></i> package in <b>R</b> is the <u><b>negative</b></u> of the shape parameter in <b><i>scipy.stats.genextreme</i></b>.

<p>This fact will be illustrated in the example below, where two methods for estimating the GEV parameters will be used.</p>

## Estimating the Three GEV Parameters

Below, we'll demonstrate how to estimate the three GEV parameters using two different approaches.  Actually, they're the same approach (Maximum Likelihood Estimation - MLE) but done the <b>Hard Way</b> (by numerical optimization of the log likelihood function) and the <b>Easy Way</b> by just calling the <i><b>fit</b></i> function of the GEV distribution (<i>genextreme</i>) in <i>scipy.stats</i>.

### Method 1: The Hard Way (numerical optimization of the log likelihood function)

Stuart Cole's book is the source for the following log likelihood function.  Note that Cole's book is also the source for the implementation of <i><b>ismev</b></i> in <b>R</b>.

If $Z_1,Z_2,...,Z_m$ are independent random variables with the GEV distribution, then given the observations, $z_i, z_2, ..., z_m$, the parameters, $\mu$, $\sigma$, and $\xi$ can be estimated using the Maximum Likelihood Method.  The log likelihood function, $\ell$, of the parameters is given below.  We need to find the values of $\mu$, $\sigma$, and $\xi$ that maximize $\ell$ for the given sample, $z_i, z_2, ..., z_m$.

$\ell(\mu,\sigma,\xi) = -m log(\sigma) - (1 + 1/\xi) \sum\limits_{i=1}^m log \left[ 1 + \xi \left( \frac {z_i - \mu} {\sigma} \right) \right] - \sum\limits_{i=1}^m \left[ 1 + \xi \left( \frac {z_i - \mu} {\sigma} \right) \right]^{-1/\xi}$ where $\xi \ne 0$ and $1 + \xi \left( \frac {z_i - \mu} {\sigma} \right) > 0$ for $i=1,...,m$

The function below implements the log likelihood function, above, and uses a standard optimization algorithm, <i>scipy.optimize.minimize</i>, to find the MLE for the EVT parameters.  Basically, it maximizes the GEV log likelihood function by minimizing its negative.


```
def gev_max_likelihood_estimate(data, starting_shape_estimate=0.1, minimization_method='nelder-mead',
                                tolerance=1e-8, display=True):
    '''Returns the maximum likelihood estimate of the three parameters of the Generalized Extreme
    Value (GEV) distribution: mu (location), sigma (scale), and xi (shape).'''
    def log_likelihood(w): # 'w' is assumed to be a tuple: (mu, sigma, xi)
        sum1 = 0.0; sum2 = 0.0
        mu = w[0]; sigma = w[1]; xi = w[2]
        for z in data:
            x = 1 + xi * ((z-mu)/sigma)
            sum1 += log(x)
            sum2 += x**(-1.0/xi)
        return -((-len(data) * log(sigma)) - (1 + 1/xi)*sum1 - sum2) # negated so we can use 'minimum'
    starting_parameter_estimate = (data.mean(), data.std(), starting_shape_estimate)
    return minimize(log_likelihood, starting_parameter_estimate, method=minimization_method,
                    options={'xtol': tolerance, 'disp': display})
```

#### Now calculate the MLE for the sea level data


```
mle = gev_max_likelihood_estimate(max_sea_levels)
```

    Optimization terminated successfully.
             Current function value: -4.339058
             Iterations: 145
             Function evaluations: 277



```
mle
```




      status: 0
        nfev: 277
     success: True
         fun: -4.3390584736794438
           x: array([ 3.87474985,  0.19804396, -0.05010954])
     message: 'Optimization terminated successfully.'
         nit: 145



#### The three parameter estimates are contained in the array, x, returned by the MLE estimation


```
mle.x
```




    array([ 3.87474985,  0.19804396, -0.05010954])




```
mu = mle.x[0]
sigma = mle.x[1]
xi = mle.x[2]
print "The mean, sigma, and shape parameters are %s, %s, and %s, resp." % (mu, sigma, xi)
```

    The mean, sigma, and shape parameters are 3.87474985416, 0.198043957296, and -0.0501095413908, resp.


### Method 2: The Easy Way - Use scipy.stats.genextreme.fit

The "fit" command estimates the MLE parameters of whatever continuous distribution you are working with in scipy.stats -- in this case, the GEV (genextreme).


```
mle2 = genextreme.fit(max_sea_levels)
mle2
```




    (0.050100209127152594, 3.874758343709805, 0.19803610557964496)



<b>Note that the shape parameter, above, is 0.0501..., whereas the shape parameter that was derived earlier was the negative of this (-0.0501...).</b>


```
mu = mle2[1]
sigma = mle2[2]
xi = mle2[0]
print "The mean, sigma, and shape parameters are %s, %s, and %s, resp." % (mu, sigma, xi)
```

    The mean, sigma, and shape parameters are 3.87475834371, 0.19803610558, and 0.0501002091272, resp.


### Plot of the estimated Probabililty Density Function (PDF) along with a normalized histogram of the data

This density (actually, its cumulative distribution function (CDF) and the CDF inverse) can be used to calculate probabilities of greater values.  But the PDF is handy to look at and compare to a normalized plot of the data.

The GEV PDF is a library function in <i>scipy.stats</i> called, <i>genextreme</i>.  All we have to do is fill in the three GEV parameters that were estimated, above, to get the PDF for our sample data.


```
def sea_levels_gev_pdf(x):
    return genextreme.pdf(x, xi, loc=mu, scale=sigma)
```


```
x = np.linspace(3.0, 6.0, num=100)
y = [sea_levels_gev_pdf(z) for z in x]
```

In the plot, below, we can see that the estimated PDF and the normalized histogram match very well.


```
figure(figsize=(12,6))
title("Probability Density (red) & Normalized Histogram (yellow)")
xlabel("Port Pirie Annual Maximum Sea Level (Meters)")
plot(x,y, color='Red')
hist(max_sea_levels, normed=1, color='Yellow')
```




    (array([ 0.82417582,  1.23626374,  1.92307692,  1.64835165,  1.23626374,
            0.96153846,  0.41208791,  0.27472527,  0.27472527,  0.13736264]),
     array([ 3.57 ,  3.682,  3.794,  3.906,  4.018,  4.13 ,  4.242,  4.354,
            4.466,  4.578,  4.69 ]),
     <a list of 10 Patch objects>)




![png](EVT_Example_files/EVT_Example_43_1.png)


### Computing Confidence Regions

A 95% one-sided confidence region can be computed by using the inverse of the cumulative distribution function (CDF) -- called PPF in scipy.stats.  So, from the calculation, below, we can say the there is only a 5% chance that the maximum sea level in given year will be above 4.42 meters, and only a 1% chance it will exceed 4.69 meters.


```
genextreme.ppf(0.95, xi, loc=mu, scale=sigma)
```




    4.4212919703314197




```
genextreme.ppf(0.99, xi, loc=mu, scale=sigma)
```




    4.6883967633028059




```
print "The maximum recorded sea level is %s meters." % max(max_sea_levels)
```

    The maximum recorded sea level is 4.69 meters.


The probability of seeing a sea level higher than the maximum recorded level is about 1%:


```
print "%4.2f percent" % ((1 - genextreme.cdf(max(max_sea_levels), xi, loc=mu, scale=sigma)) * 100.0)
```

    0.99 percent


## Run the example in R using the package 'ismev'

The <i>rmagic</i> extension contains IPython magics for working with R via the <i>rpy2</i> module.


```
%load_ext rmagic
```

    The rmagic extension is already loaded. To reload it, use:
      %reload_ext rmagic


The following cell runs the GEV MLE fit in R using the data Port Pirie data file contained in 'ismev'.

<p>The result of the fit is output to a numpy array.  The graphics output by the command <i>gen.diag</i> is saved to a jpg file and then loaded as an image.</p>


```
%%R -o r_fit
library('ismev')
data(portpirie) # Data here is NOT pushed from Python; it is a dataframe in 'ismev'
r_fit <- gev.fit(portpirie[,2])
jpeg('~/r_gev_diag.jpg')
gev.diag(r_fit)
dev.off()
```


    $conv
    [1] 0
    
    $nllh
    [1] -4.339058
    
    $mle
    [1]  3.87474692  0.19804120 -0.05008773
    
    $se
    [1] 0.02793211 0.02024610 0.09825633
    



The 6th element of the array output by R is the set of GEV parameters we are seeking:


```
r_fit_results_in_py = array(r_fit[6])
r_fit_results_in_py
```




    array([ 3.87474692,  0.1980412 , -0.05008773])



And here's the image that was created in R, above:


```
from IPython.core.display import Image 
homedir = !echo $HOME
Image(filename= homedir[0] + '/r_gev_diag.jpg')
```




![jpeg](EVT_Example_files/EVT_Example_59_0.jpeg)



<b><u>TO BE DONE</u></b>
<p>1. Don't use the input data from the R 'ismev' library.  Instead, push the Python input data to R to do the analysis.</p>
<p>2. Also, see if the graphics output by R can be pushed directly to Python, without saving to a file first.</p>
