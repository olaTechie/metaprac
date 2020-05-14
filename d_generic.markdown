---
layout: page
title: Generic meta
# permalink: /generic/
---

# GENERIC DATA

**Learning outcomes**:

You will have _knowledge_ about and be able:

- Conduct fixed and random effects meta-analysis
- Measure effect size
- Explore heterogeneity using sub-group and meta-regression analyses
- Explore and address small study effects using funnel plot and trim and fill

STEPS in conducting **Generic Data** meta-analysis

1. STEP 1 - LOAD DATA
2. STEP 2 - DECLARE, UPDATE & DESCRIBE meta data
3. STEP 3 - SUMMARIZE meta data by using a TABLE or a FOREST PLOT
4. STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis
5. STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS

# Link to YouTube Video Lecture

**Click the image below:**

<a href="http://www.youtube.com/watch?feature=player_embedded&v=bTNgi4bpMP0" target="_blank"><img src="http://img.youtube.com/vi/bTNgi4bpMP0/0.jpg" alt="04-Generic Data MetaAnalysis" width="1200" height="800" border="10" /></a>

## STEP 1 - LOAD DATA

The meta command accepts effect sizes and confidence intervals, not just count data. By specifying two variables after commands, you indicate to Stata that the variables represent effect sizes and standard error.

Below are results from trials that examined effect of exercise on depression.

Contains data from Exercise for depression

- Id: ID no. of study
- study: First author of study
- smd: Standardised mean difference
- varsmd: Var(smd)
- sesmd: SE(smd)
- abstract: Published as abstract?
- duration: Duration of follow-up (weeks)
- itt: Intention-to-treat analysis?
- alloc: Allocation concealment adequate?
- Phd: Published as PhD thesis?

**Option 1**
Copy and paste from Excel (see attached exercise4deprsn.xlsx below)

---

`edit`

**Option 2**
Input raw directly into Stata (using ‘do file editor’ type `doedit`)

`clear all input id str20 study smd varsmd sesmd abstract duration itt alloc phd 1 Mutrie -2.53 0.16 0.4 1 4 0 0 0 2 McNeil -1.07 0.1681 0.41 0 6 0 0 0 3 Reuter -2.1 0.16 0.4 1 10 0 0 0 4 Doyne -1.2 0.1849 0.43 0 8 0 0 0 5 Hess-Homeier -0.82 0.3249 0.57 0 8 0 0 1 6 Epstein -0.84 0.2116 0.46 0 8 0 0 1 7 Martinsen -1.16 0.0784 0.28 0 9 0 1 0 8 Singh -0.45 0.1156 0.34 0 10 1 1 0 9 Klein 0.25 0.2601 0.51 0 12 0 0 0 10 Veale -0.53 0.0576 0.24 0 12 0 1 0 end`

---

**Option 3**
Load Stata data file directly from Excel file

---

```python
import excel  using data/exercise4deprsn.xlsx, firstrow clear
describe
```

    (10 vars, 10 obs)


    Contains data
      obs:            10
     vars:            10
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    id              byte    %10.0g                id
    study           str12   %12s                  study
    smd             double  %10.0g                smd
    varsmd          double  %10.0g                varsmd
    sesmd           double  %10.0g                sesmd
    abstract        byte    %10.0g                abstract
    duration        byte    %10.0g                duration
    itt             byte    %10.0g                itt
    alloc           byte    %10.0g                alloc
    phd             byte    %10.0g                phd
    --------------------------------------------------------------------------------
    Sorted by:
         Note: Dataset has changed since last saved.

**Option 4**
Load Stata data file directly (if already saved)

---

```python
use data/exercise4deprsn.dta, clear

describe
```

    (Excercise for depression)


    Contains data from data/exercise4deprsn.dta
      obs:            10                          Excercise for depression
     vars:            10                          11 Oct 2015 21:16
                                                  (_dta has notes)
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    id              byte    %3.0f               * ID no. of study
    study           str12   %-12s                 First author of study
    smd             float   %6.2f                 Standardised mean difference
    varsmd          float   %7.4f                 Var(smd)
    sesmd           float   %9.0g                 SE(smd)
    abstract        byte    %-8.0g     noyes      Published as abstract?
    duration        byte    %8.0g                 Duration of follow-up (weeks)
    itt             byte    %-8.0g     noyes      Intention-to-treat analysis?
    alloc           byte    %-8.0g     noyes      Allocation concealment adequate?
    phd             byte    %-8.0g     noyes      Published as PhD thesis?
                                                * indicated variables have notes
    --------------------------------------------------------------------------------
    Sorted by: id

## STEP 2- DECLARE, UPDATE & DESCRIBE meta data

```python
meta set smd sesmd, studylabel(study) eslabel(Std. Mean Diff.)
```

    Meta-analysis setting information

     Study information
        No. of studies:  10
           Study label:  study
            Study size:  N/A

           Effect size
                  Type:  Generic
                 Label:  Std. Mean Diff.
              Variable:  smd

             Precision
             Std. Err.:  sesmd
                    CI:  [_meta_cil, _meta_ciu]
              CI level:  95%

      Model and method
                 Model:  Random-effects
                Method:  REML

## STEP 3- SUMMARIZE meta data by using a TABLE or a FOREST PLOT

_Now, combine the results of trials, using the fixed- and random effects model_

---

> _Q2.1 - What are the summary estimates and 95% CI for both fixed and random-effects_

> _Q2.2 - Are the results homogenous?_

---

```python
meta forestplot, nullrefline fixed
graph display
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd
            Study label:  study

<img src="./assets/images/04_GenericData_12_1.svg"  width="1200" height="800">

```python
meta forestplot, nullrefline random
graph display
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd
            Study label:  study

<img src="./assets/images/04_GenericData_13_1.svg"  width="1200" height="800">

## STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis

_Examine the pooled estimates differ according to following study characteristics:_

- Publication type
- Intention to treat analysis
- Allocation concealment
- Published as PhD thesis or not

```python
meta forestplot, nullrefline random subgroup(abstract)
graph display
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd
            Study label:  study

<img src="./assets/images/04_GenericData_15_1.svg"  width="1200" height="800">

```python
meta forestplot, nullrefline random subgroup(itt)
graph display
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd
            Study label:  study

<img src="./assets/images/04_GenericData_16_1.svg"  width="1200" height="800">

```python
meta forestplot, nullrefline random subgroup(alloc)
graph display
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd
            Study label:  study

<img src="./assets/images/04_GenericData_17_1.svg"  width="1200" height="800">

```python
meta forestplot, nullrefline random subgroup(phd)
graph display
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd
            Study label:  study

<img src="./assets/images/04_GenericData_18_1.svg"  width="1200" height="800">

### Univariable meta-regression

Fit a meta-regression model that explains the heterogeneity in terms of study-level covariates.

---

> _Q2.3 - Which study level covariates are significant?_

> _Q2.4 - What percentage of heterogeneity is explained for these covariates?_

---

```python
meta regress abstract
meta regress duration
estat bubbleplot
graph display
meta regress itt
meta regress alloc
meta regress phd
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =  .03269
                                                                  I2 (%) =   19.02
                                                                      H2 =    1.23
                                                           R-squared (%) =   92.74
                                                        Wald chi2(1)   =     20.78
                                                        Prob > chi2    =    0.0000
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        abstract |  -1.564109   .3431559    -4.56   0.000    -2.236682   -.8915357
           _cons |   -.750891   .1463283    -5.13   0.000    -1.037689   -.4640929
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) =  9.94    Prob > Q_res = 0.2690


      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .2019
                                                                  I2 (%) =   58.10
                                                                      H2 =    2.39
                                                           R-squared (%) =   55.16
                                                        Wald chi2(1)   =      7.02
                                                        Prob > chi2    =    0.0081
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        duration |   .2097633    .079171     2.65   0.008     .0545909    .3649357
           _cons |  -2.907511   .7239578    -4.02   0.000    -4.326442    -1.48858
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) = 18.11    Prob > Q_res = 0.0204

<img src="./assets/images/04_GenericData_20_1.svg"  width="1200" height="800">

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .4719
                                                                  I2 (%) =   76.66
                                                                      H2 =    4.28
                                                           R-squared (%) =    0.00
                                                        Wald chi2(1)   =      0.70
                                                        Prob > chi2    =    0.4028
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             itt |   .6790167   .8116419     0.84   0.403    -.9117722    2.269806
           _cons |  -1.129017   .2668869    -4.23   0.000    -1.652105    -.605928
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) = 32.33    Prob > Q_res = 0.0001


      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .4383
                                                                  I2 (%) =   75.10
                                                                      H2 =    4.02
                                                           R-squared (%) =    2.64
                                                        Wald chi2(1)   =      1.01
                                                        Prob > chi2    =    0.3142
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           alloc |   .5186852   .5153992     1.01   0.314    -.4914787    1.528849
           _cons |  -1.235379   .3032112    -4.07   0.000    -1.829662   -.6410955
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) = 28.46    Prob > Q_res = 0.0004


      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .5089
                                                                  I2 (%) =   79.16
                                                                      H2 =    4.80
                                                           R-squared (%) =    0.00
                                                        Wald chi2(1)   =      0.16
                                                        Prob > chi2    =    0.6910
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             phd |   .2718996   .6840508     0.40   0.691    -1.068815    1.612615
           _cons |  -1.102629   .2853771    -3.86   0.000    -1.661957   -.5432998
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) = 35.15    Prob > Q_res = 0.0000

```python
foreach factor of varlist abstract duration itt alloc phd {
	meta regress  `factor'
}
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =  .03269
                                                                  I2 (%) =   19.02
                                                                      H2 =    1.23
                                                           R-squared (%) =   92.74
                                                        Wald chi2(1)   =     20.78
                                                        Prob > chi2    =    0.0000
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        abstract |  -1.564109   .3431559    -4.56   0.000    -2.236682   -.8915357
           _cons |   -.750891   .1463283    -5.13   0.000    -1.037689   -.4640929
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) =  9.94    Prob > Q_res = 0.2690

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .2019
                                                                  I2 (%) =   58.10
                                                                      H2 =    2.39
                                                           R-squared (%) =   55.16
                                                        Wald chi2(1)   =      7.02
                                                        Prob > chi2    =    0.0081
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        duration |   .2097633    .079171     2.65   0.008     .0545909    .3649357
           _cons |  -2.907511   .7239578    -4.02   0.000    -4.326442    -1.48858
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) = 18.11    Prob > Q_res = 0.0204

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .4719
                                                                  I2 (%) =   76.66
                                                                      H2 =    4.28
                                                           R-squared (%) =    0.00
                                                        Wald chi2(1)   =      0.70
                                                        Prob > chi2    =    0.4028
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             itt |   .6790167   .8116419     0.84   0.403    -.9117722    2.269806
           _cons |  -1.129017   .2668869    -4.23   0.000    -1.652105    -.605928
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) = 32.33    Prob > Q_res = 0.0001

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .4383
                                                                  I2 (%) =   75.10
                                                                      H2 =    4.02
                                                           R-squared (%) =    2.64
                                                        Wald chi2(1)   =      1.01
                                                        Prob > chi2    =    0.3142
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           alloc |   .5186852   .5153992     1.01   0.314    -.4914787    1.528849
           _cons |  -1.235379   .3032112    -4.07   0.000    -1.829662   -.6410955
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) = 28.46    Prob > Q_res = 0.0004

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .5089
                                                                  I2 (%) =   79.16
                                                                      H2 =    4.80
                                                           R-squared (%) =    0.00
                                                        Wald chi2(1)   =      0.16
                                                        Prob > chi2    =    0.6910
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             phd |   .2718996   .6840508     0.40   0.691    -1.068815    1.612615
           _cons |  -1.102629   .2853771    -3.86   0.000    -1.661957   -.5432998
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(8) = 35.15    Prob > Q_res = 0.0000

### Multivariale meta-regression

```python
meta regress abstract duration
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Random-effects meta-regression                      Number of obs  =        10
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 = 5.3e-08
                                                                  I2 (%) =    0.00
                                                                      H2 =    1.00
                                                           R-squared (%) =  100.00
                                                        Wald chi2(2)   =     30.61
                                                        Prob > chi2    =    0.0000
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        abstract |  -1.243886   .3412327    -3.65   0.000     -1.91269    -.575082
        duration |   .1207103   .0533561     2.26   0.024     .0161343    .2252863
           _cons |  -1.916086   .5312603    -3.61   0.000    -2.957337   -.8748354
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(7) =  4.82    Prob > Q_res = 0.6813

## STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS

---

> _Q2.5 - Examine whether there is evidence of publication bias?_

---

```python
meta funnelplot, metric(invse)
graph display
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd
                  Model:  Common-effect
                 Method:  Inverse-variance

<img src="./assets/images/04_GenericData_25_1.svg"  width="1200" height="800">

```python
meta bias, egger
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Regression-based Egger test for small-study effects
    Random-effects model
    Method: REML

    H0: beta1 = 0; no small-study effects
                beta1 =      0.48
          SE of beta1 =     2.804
                    z =      0.17
           Prob > |z| =    0.8641

```python
meta trimfill
```

      Effect-size label:  Std. Mean Diff.
            Effect size:  smd
              Std. Err.:  sesmd

    Nonparametric trim-and-fill analysis of publication bias
    Linear estimator, imputing on the right

    Iteration                            Number of studies =     10
      Model: Random-effects                       observed =     10
     Method: REML                                  imputed =      0

    Pooling
      Model: Random-effects
     Method: REML

    ---------------------------------------------------------------
                 Studies |  Std. Mean Diff.    [95% Conf. Interval]
    ---------------------+-----------------------------------------
                Observed |           -1.056      -1.541      -0.570
      Observed + Imputed |           -1.056      -1.541      -0.570
    ---------------------------------------------------------------

# GENERIC DATA

STEPS in conducting **Generic Data** meta-analysis

1. STEP 1 - LOAD DATA
2. STEP 2 - DECLARE, UPDATE & DESCRIBE meta data
3. STEP 3 - SUMMARIZE meta data by using a TABLE or a FOREST PLOT
4. STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis
5. STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS
