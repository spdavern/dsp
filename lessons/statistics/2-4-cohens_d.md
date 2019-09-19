[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

Exercise 2.4 Using the variable totalwgt_lb, investigate whether first babies are lighter or heavier than others. Compute Cohen’s d to quantify the difference between the groups. How does it compare to the difference in pregnancy length?

>> The computed difference in average weight between first babies (7 lb 3 oz, n=4,363) and the others (7 lb 5 oz, n=4,675) is 2 oz which is just less than 0.09 standard deviations different (Cohen's d).  This is 3 times larger than Cohen's d for the difference in pregnancy length (1/2 day longer).  The p-value (2.6e-5) testing for equivalence in average weights suggests the difference, while small is statistically significant.

Below is a condensed collection of code that reproduces the above results:

```python
# Source: http://thinkstats.com, Allen B. Downey, 2016
from __future__ import print_function, division
import nsfg
import first
import thinkstats2

# Standard libraries
import numpy as np
import scipy.stats as stats

def CohenEffectSize(group1, group2):
    """Computes Cohen's effect size for two groups.
    
    group1: Series or DataFrame
    group2: Series or DataFrame
    
    returns: float if the arguments are Series;
             Series if the arguments are DataFrames
    """
    diff = group1.mean() - group2.mean()

    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)

    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    d = diff / np.sqrt(pooled_var)
    return d

# Read in data, use live births only, divide into first babies
# and all the others
preg = nsfg.ReadFemPreg()
live = preg[preg.outcome == 1]
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]

# Pregnancy length difference calculations:
print('pregnancy length group means: firsts, others, difference in days')
print(firsts.prglngth.mean(), 
      others.prglngth.mean(), 
      (firsts.prglngth.mean()-others.prglngth.mean())*7)
CES_prglngth = CohenEffectSize(firsts.prglngth,others.prglngth)
print('CES:',CES_prglngth)

# Baby weight difference calculations:
print('Baby weight(lb) group means: firsts, others, difference(oz):')
print(firsts.totalwgt_lb.mean(), 
      others.totalwgt_lb.mean(), 
      (others.totalwgt_lb.mean()- firsts.totalwgt_lb.mean())*16)
CES_totwgt_lb = CohenEffectSize(firsts.totalwgt_lb,others.totalwgt_lb)
print('CES:',CES_totwgt_lb)
print('Decimal portions of weights in oz: firsts, other')
print((firsts.totalwgt_lb.mean()-np.floor(firsts.totalwgt_lb.mean()))*16, 
      (others.totalwgt_lb.mean()-np.floor(others.totalwgt_lb.mean()))*16)
print('Baby weight group sizes: firsts, others:')
print(len(firsts[firsts.totalwgt_lb.notnull()]),
      len(others[others.totalwgt_lb.notnull()]))
print('t-test between group means of baby weights:')
print(stats.ttest_ind(a=firsts.totalwgt_lb,
                b=others.totalwgt_lb,
                nan_policy='omit'))

# Comparison of weight and pregnancy length CES's:
print(CES_totwgt_lb/ CES_prglngth)
```

