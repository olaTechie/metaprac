---
layout: page
title: Prevalence meta
# permalink: /prevalence/
---

# PREVALENCE OR PROPORTION DATA

## Learning outcomes:

You will have _knowledge_ about and be able:

- Conduct fixed and random effects meta-analysis
- Measure effect size HETEROGENEITY
- Explore heterogeneity using sub-group and meta-regression analyses
- Explore and address small study effects using funnel plot and trim and fill

STEPS in conducting **Prevalence Data**

1. STEP 1 - LOAD DATA
2. STEP 2 - DECLARE, UPDATE & DESCRIBE meta data
3. STEP 3 - SUMMARIZE meta data by using a TABLE or a FOREST PLOT
4. STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis
5. STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS

## Materials and setup

Laptop users: you will need a copy of Stata installed on your machine.

- You can install a licensed version from https://warwick.ac.uk/services/its/servicessupport/software/list/stata
- Find class materials at <a href="./assets/PracMaterials.zip" download>Click to Download</a>
- Download and extract to your desktop or any folder of your choice!

# Link to YouTube Video Lecture

**Click the image below:**

<a href="http://www.youtube.com/watch?feature=player_embedded&v=4IFCsymBHn4" target="_blank"><img src="http://img.youtube.com/vi/4IFCsymBHn4/0.jpg" alt="06-Prevalence Data Meta-Analysis" width="1200" height="800" border="10" /></a>

## STEP 1 - LOAD DATA

_The data is from a meta-analysis that estimated the prevalence of underlying disorders in hospitalized COVID-19 patients_

```python
use data/covid-premorb.dta, clear

describe
```

    Contains data from data/covid-premorb.dta
      obs:            40
     vars:             5                          10 May 2020 11:34
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    study           str26   %-9s
    morb            long    %12.0g     morb
    r               double  %10.0g
    n               double  %10.0g
    male            double  %10.0g
    --------------------------------------------------------------------------------
    Sorted by:

```python
tab morb
```

           morb |      Freq.     Percent        Cum.
    ------------+-----------------------------------
            CKD |          7       17.50       17.50
            HTN |          7       17.50       35.00
             DM |          6       15.00       50.00
            Mal |          7       17.50       67.50
           COPD |          5       12.50       80.00
            CVD |          8       20.00      100.00
    ------------+-----------------------------------
          Total |         40      100.00

## STEP 2- DECLARE, UPDATE & DESCRIBE meta data

_Not applicable initially_

## STEP 3- SUMMARIZE meta data by using a TABLE or a FOREST PLOT

```python
metaprop r n, ftt random notable
graph display
```

<img src="./assets/images/06_PrevalenceData_8_0.svg"  width="1200" height="800">

```python
metaprop r n, ftt random label(namevar=study)  notable
graph display
```

<img src="./assets/images/06_PrevalenceData_9_0.svg"  width="1200" height="800">

```python
metaprop r n, ftt random notable label(namevar=study) power(2)
graph display
```

<img src="./assets/images/06_PrevalenceData_10_0.svg"  width="1200" height="800">

## STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis

---

> _Q4.1 - What is the commonest pre-morb condition?_

> _Q4.2 - What is the least common pre-mord condition?_

---

```python
metaprop r n, ftt random label(namevar=study) power(2) by(morb)
graph di
```

               Study     |      ES       [95% Conf. Interval]  % Weight
    ---------------------+---------------------------------------------------
         CKD
    Chaolin Huang, et al |        7.32      2.52        19.43        2.20
    Nanshan Chen, et al. |        3.03      1.04         8.53        2.53
    Dawei Wang, et al. ( |        2.90      1.13         7.22        2.61
    Jie.Li, et al. (36)  |       47.06     26.17        69.04        1.68
    Wei-Jie Guan, et al. |        0.09      0.02         0.51        2.81
    Jin-jin Zhang, et al |        1.43      0.39         5.06        2.62
    Jian Wu, et al. (39) |        1.25      0.22         6.75        2.47
     Sub-total           |
      Random pooled  ES  |        3.61      0.44         8.90       16.93
    ---------------------+---------------------------------------------------
         HTN
    Chaolin Huang, et al |       14.63      6.88        28.44        2.20
    Dawei Wang, et al. ( |       31.16     24.03        39.31        2.61
    Jie.Li, et al. (36)  |        5.88      1.05        26.98        1.68
    Wei-Jie Guan, et al. |       14.92     12.94        17.15        2.81
    Xiao-Wei Xu, et al.  |        8.06      3.49        17.53        2.38
    Jin-jin Zhang, et al |       30.00     23.03        38.04        2.62
    Kui L, et al. (40)   |        9.49      5.63        15.56        2.61
     Sub-total           |
      Random pooled  ES  |       16.37     10.15        23.65       16.92
    ---------------------+---------------------------------------------------
         DM
    Chaolin Huang, et al |       19.51     10.23        34.01        2.20
    Dawei Wang, et al. ( |       10.14      6.14        16.31        2.61
    Wei-Jie Guan, et al. |        7.37      5.97         9.07        2.81
    Xiao-Wei Xu, et al.  |        1.61      0.29         8.59        2.38
    Jin-jin Zhang, et al |       12.14      7.72        18.59        2.62
    Kui L, et al. (40)   |       10.22      6.19        16.42        2.61
     Sub-total           |
      Random pooled  ES  |        9.03      5.92        12.67       15.23
    ---------------------+---------------------------------------------------
         Mal
    Chaolin Huang, et al |        2.44      0.43        12.60        2.20
    Nanshan Chen, et al. |        1.01      0.18         5.50        2.53
    Dawei Wang, et al. ( |        7.25      3.98        12.83        2.61
    Wei-Jie Guan, et al. |        0.91      0.49         1.67        2.81
    Wenhua Liang,et al.  |        1.13      0.72         1.78        2.82
    Jian Wu, et al. (39) |        1.25      0.22         6.75        2.47
    Kui L, et al. (40)   |        1.46      0.40         5.17        2.61
     Sub-total           |
      Random pooled  ES  |        1.50      0.58         2.73       18.06
    ---------------------+---------------------------------------------------
         COPD
    Chaolin Huang, et al |        2.44      0.43        12.60        2.20
    Dawei Wang, et al. ( |        2.90      1.13         7.22        2.61
    Wei-Jie Guan, et al. |        1.09      0.63         1.90        2.81
    Xiao-Wei Xu, et al.  |        1.61      0.29         8.59        2.38
    Jin-jin Zhang, et al |        1.43      0.39         5.06        2.62
     Sub-total           |
      Random pooled  ES  |        0.95      0.43         1.61       12.62
    ---------------------+---------------------------------------------------
         CVD
    Chaolin Huang, et al |       14.63      6.88        28.44        2.20
    Nanshan Chen, et al. |       40.40     31.27        50.25        2.53
    Dawei Wang, et al. ( |       14.49      9.58        21.33        2.61
    Wei-Jie Guan, et al. |        2.46      1.69         3.55        2.81
    Xiao-Wei Xu, et al.  |        1.61      0.29         8.59        2.38
    Jin-jin Zhang, et al |        5.00      2.44         9.96        2.62
    Jian Wu, et al. (39) |       31.25     22.15        42.07        2.47
    Kui L, et al. (40)   |        7.30      4.01        12.92        2.61
     Sub-total           |
      Random pooled  ES  |       12.11      4.40        22.75       20.24
    ---------------------+---------------------------------------------------
    Overall              |
      Random pooled  ES  |        6.83      4.52         9.54      100.00
    ---------------------+---------------------------------------------------

    Test(s) of heterogeneity:
                   Heterogeneity  degrees of
                     statistic     freedom            P       I^2**
    CKD                     60.52      6            0.00     90.09%
    HTN                     44.18      6            0.00     86.42%
    DM                      15.22      5            0.01     67.15%
    Mal                     17.60      6            0.01     65.91%
    COPD                     3.92      4            0.42      0.00%
    CVD                    170.42      7            0.00     95.89%
    Overall                908.00     39            0.00     95.70%
    ** I^2: the variation in ES attributable to heterogeneity)


    Random: Test for heterogeneity between sub-groups:
                            79.93      5            0.00

    Significance test(s) of  ES=0

    CKD                   z= 2.72       p = 0.01
    HTN                   z= 7.89       p = 0.00
    DM                    z= 8.95       p = 0.00
    Mal                   z= 4.49       p = 0.00
    COPD                  z= 5.21       p = 0.00
    CVD                   z= 4.37       p = 0.00
    Overall               z= 8.99       p = 0.00
    -------------------------------------------------------------------------

<img src="./assets/images/06_PrevalenceData_12_1.svg"  width="1200" height="800">

---

> _Q4.3 - Is there is correlation between the prevalence estimates and % males_

> _Q4.4 - Are smaller studies tended to have reported over reported the prevalence estimates_

---

```python
d
```

    Contains data from data/covid-premorb.dta
      obs:            40
     vars:            10                          10 May 2020 11:34
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    study           str26   %-9s
    morb            long    %12.0g     morb
    r               double  %10.0g
    n               double  %10.0g
    male            double  %10.0g
    _ES             float   %9.0g                 ES
    _seES           float   %9.0g                 se(ES)
    _LCI            float   %9.0g                 Lower CI (ES)
    _UCI            float   %9.0g                 Upper CI (ES)
    _WT             float   %9.0g                 Random weight
    --------------------------------------------------------------------------------
    Sorted by:

```python
* STEP 2- DECLARE, UPDATE & DESCRIBE meta data
meta set _ES _seES, studylabel(study) eslabel(Premorb. Prevalence.)
```

    Meta-analysis setting information

     Study information
        No. of studies:  40
           Study label:  study
            Study size:  N/A

           Effect size
                  Type:  Generic
                 Label:  Premorb. Prevalence.
              Variable:  _ES

             Precision
             Std. Err.:  _seES
                    CI:  [_meta_cil, _meta_ciu]
              CI level:  95%

      Model and method
                 Model:  Random-effects
                Method:  REML

```python
meta regress male
estat bubbleplot
graph di
```

      Effect-size label:  Premorb. Prevalence.
            Effect size:  _ES
              Std. Err.:  _seES

    Random-effects meta-regression                      Number of obs  =        39
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 = .003317
                                                                  I2 (%) =   44.21
                                                                      H2 =    1.79
                                                           R-squared (%) =    0.00
                                                        Wald chi2(1)   =      0.04
                                                        Prob > chi2    =    0.8488
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            male |    .000455    .002387     0.19   0.849    -.0042234    .0051334
           _cons |   .0539036   .1356548     0.40   0.691     -.211975    .3197822
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(37) = 62.24   Prob > Q_res = 0.0058

<img src="./assets/images/06_PrevalenceData_16_1.svg"  width="1200" height="800">

```python
meta regress n
estat bubbleplot
graph di
```

      Effect-size label:  Premorb. Prevalence.
            Effect size:  _ES
              Std. Err.:  _seES

    Random-effects meta-regression                      Number of obs  =        40
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 = .002014
                                                                  I2 (%) =   34.33
                                                                      H2 =    1.52
                                                           R-squared (%) =   34.55
                                                        Wald chi2(1)   =      5.07
                                                        Prob > chi2    =    0.0243
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
               n |  -.0000583   .0000259    -2.25   0.024    -.0001091   -7.57e-06
           _cons |   .1078519   .0215803     5.00   0.000     .0655554    .1501484
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(38) = 55.54   Prob > Q_res = 0.0329

<img src="./assets/images/06_PrevalenceData_17_1.svg"  width="1200" height="800">

## STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS

```python
meta funnelplot, metric(invse)
graph display
```

      Effect-size label:  Premorb. Prevalence.
            Effect size:  _ES
              Std. Err.:  _seES
                  Model:  Common-effect
                 Method:  Inverse-variance

<img src="./assets/images/06_PrevalenceData_19_1.svg"  width="1200" height="800">

```python
meta bias, egger
```

      Effect-size label:  Premorb. Prevalence.
            Effect size:  _ES
              Std. Err.:  _seES

    Regression-based Egger test for small-study effects
    Random-effects model
    Method: REML

    H0: beta1 = 0; no small-study effects
                beta1 =      0.72
          SE of beta1 =     0.353
                    z =      2.05
           Prob > |z| =    0.0403

```python
meta trimfill
```

      Effect-size label:  Premorb. Prevalence.
            Effect size:  _ES
              Std. Err.:  _seES

    Nonparametric trim-and-fill analysis of publication bias
    Linear estimator, imputing on the left

    Iteration                            Number of studies =     40
      Model: Random-effects                       observed =     40
     Method: REML                                  imputed =      0

    Pooling
      Model: Random-effects
     Method: REML

                    theta: Overall Premorb. Prevalence.
    ---------------------------------------------------------------
                 Studies |            theta    [95% Conf. Interval]
    ---------------------+-----------------------------------------
                Observed |            0.075       0.044       0.105
      Observed + Imputed |            0.075       0.044       0.105
    ---------------------------------------------------------------

# PREVALENCE OR PROPORTION DATA

STEPS in conducting **Prevalence Data**

1. STEP 1 - LOAD DATA
2. STEP 2 - DECLARE, UPDATE & DESCRIBE meta data
3. STEP 3 - SUMMARIZE meta data by using a TABLE or a FOREST PLOT
4. STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis
5. STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS

## Sections

1. [Overview meta-analysis data types](index.markdown)
2. [Introduction to Stata](b_stata.markdown)
3. [Meta-analysis of binary data](c_binary.markdown)
4. [Meta-analysis of generic data](d_generic.markdown)
5. [Meta-analysis of continuous data](e_continuous.markdown)
6. [Meta-analysis of prevalence data](f_prevalence.markdown)
