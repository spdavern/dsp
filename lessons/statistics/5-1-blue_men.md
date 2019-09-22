[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)



**Exercise 5.1** In the BRFSS (see Section 5.4), the distribution of heights is
roughly normal with parameters µ = 178 cm and σ = 7.7 cm for men, and
µ = 163 cm and σ = 7.3 cm for women.

In order to join Blue Man Group, you have to be male between 5’10” and
6’1” (see http://bluemancasting.com). What percentage of the U.S. male
population is in this range? Hint: use `scipy.stats.norm.cdf`.

Convert heights from ft and in to cm:

```python
h1 = (5*12 +10)*2.54
h2 = (6*12 + 1)*2.54
h1, h2
```

Then use the cumulative density function to determine the fraction of male population less than or equal to each height. The difference between the two is the fraction of the male population between these heights:

```python
mu, scale = 178, 7.7
cdf_h1 = scipy.stats.norm.cdf(h1, loc=mu, scale=scale)
cdf_h2 = scipy.stats.norm.cdf(h2, loc=mu, scale=scale)
print('Fraction of U.S. males that have height to be eligible to join Blue Man Group: {0:1.1f}'
      .format((cdf_h2-cdf_h1)*100),'%')
```

Solution:

Fraction of U.S. males that have height to be eligible to join Blue Man Group: 34.3 %

The above content is implemented in the juypter notebook [chap05.ex1.ipynb](https://github.com/spdavern/dsp/blob/master/lessons/statistics/code/chap05ex1.ipynb).

