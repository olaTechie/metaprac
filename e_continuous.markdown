---
layout: page
title: Continuous meta
# permalink: /continuous/
---

# CONTINUOUS DATA

**Learning outcomes**:

You will have _knowledge_ about and be able:

- Conduct fixed and random effects meta-analysis
- Measure effect size
- Explore heterogeneity using sub-group and meta-regression analyses
- Explore and address small study effects using funnel plot and trim and fill

STEPS in conducting **Continuous Data**

1. STEP 1 - LOAD DATA
2. STEP 2 - DECLARE, UPDATE & DESCRIBE meta data
3. STEP 3 - SUMMARIZE meta data by using a TABLE or a FOREST PLOT
4. STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis
5. STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS

# Link to YouTube Video Lecture

**Click the image below:**

<a href="http://www.youtube.com/watch?feature=player_embedded&v=5UUQN_7sGTs" target="_blank"><img src="http://img.youtube.com/vi/5UUQN_7sGTs/0.jpg" alt="05-Continuous Data Meta-Analysis" width="1200" height="800" border="10" /></a>

## STEP 1 - LOAD DATA

The meta command can also handle continuous outcomes (six variables).  
`n1 mean1 sd1 n2 mean2 sd2`

Data for the continuous ‘outcome’:

- year: Year
- tsample: Number treated (exposed)
- tmean: Mean in treated (exposed)
- tsd: Standard deviation in treated (exposed)
- csample: Number not treated (unexposed)
- cmean: Mean in untreated (unexposed)
- csd: Standard deviation in untreated (unexposed)

**Option 1**
Copy and paste from Excel (see attached cont_data.xlsx below)

---

`edit`

**Option 2**
Input raw directly into Stata (using ‘do file editor’ type `doedit`)

**Option 3**
Load Stata data file directly from Excel file

---

```python
import excel  using data/cont_data.xlsx, firstrow clear
describe
```

    (8 vars, 20 obs)


    Contains data
      obs:            20
     vars:             8
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    id              str14   %14s                  id
    year            int     %10.0g                year
    tsample         int     %10.0g                tsample
    tmean           double  %10.0g                tmean
    tsd             double  %10.0g                tsd
    csample         int     %10.0g                csample
    cmean           double  %10.0g                cmean
    csd             double  %10.0g                csd
    --------------------------------------------------------------------------------
    Sorted by:
         Note: Dataset has changed since last saved.

**Option 4**
Load Stata data file directly (if already saved)

---

```python
use data/cont_data.dta, clear

describe
```

    (Example dataset for meta-analyses, Ross Harris 2006)


    Contains data from data/cont_data.dta
      obs:            20                          Example dataset for
                                                    meta-analyses, Ross Harris 2006
     vars:             8                          11 Oct 2015 21:15
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    id              str14   %14s                  Study identifier
    year            int     %8.0g                 Year
    tsample         int     %9.0g                 Number treated (exposed)
    tmean           float   %9.0g                 Mean in treated (exposed)
    tsd             float   %9.0g                 Standard deviation in treated
                                                    (exposed)
    csample         int     %9.0g                 Number not treated (unexposed)
    cmean           float   %9.0g                 Mean in untreated (unexposed)
    csd             float   %9.0g                 Standard deviation in untreated
                                                    (unexposed)
    --------------------------------------------------------------------------------
    Sorted by: year

## STEP 2- DECLARE, UPDATE & DESCRIBE meta data

| Continuous Type | Description                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------ |
| hedgesg         | Hedges's g standardized mean difference; the default                                                         |
| cohend          | Cohen's d standardized mean difference                                                                       |
| glassdelta2     | Glass's ∆ mean difference standardized by group 2 (control) standard deviation; more common than glassdelta1 |
| glassdelta1     | Glass's ∆ mean difference standardized by group 1 (treatment) standard deviation                             |
| mdiff           | (unstandardized) mean difference                                                                             |

```python
meta esize tsample tmean tsd csample cmean csd, studylabel(id) esize(hedgesg)
```

    >

    Meta-analysis setting information

     Study information
        No. of studies:  20
           Study label:  id
            Study size:  _meta_studysize
          Summary data:  tsample tmean tsd csample cmean csd

           Effect size
                  Type:  hedgesg
                 Label:  Hedges's g
              Variable:  _meta_es
       Bias correction:  Approximate

             Precision
             Std. Err.:  _meta_se
        Std. Err. adj.:  None
                    CI:  [_meta_cil, _meta_ciu]
              CI level:  95%

      Model and method
                 Model:  Random-effects
                Method:  REML

## STEP 3- SUMMARIZE meta data by using a TABLE or a FOREST PLOT

```python
meta forestplot, nullrefline
graph display
```

      Effect-size label:  Hedges's g
            Effect size:  _meta_es
              Std. Err.:  _meta_se
            Study label:  id

<img src="./assets/images/05_ContinuousData_12_1.svg"  width="1200" height="800">

```python
meta update, esize(mdiff)
```

    -> meta esize tsample tmean tsd csample cmean csd , esize(mdiff) studylabel(id)

    Meta-analysis setting information from meta esize

     Study information
        No. of studies:  20
           Study label:  id
            Study size:  _meta_studysize
          Summary data:  tsample tmean tsd csample cmean csd

           Effect size
                  Type:  mdiff
                 Label:  Mean Diff.
              Variable:  _meta_es

             Precision
             Std. Err.:  _meta_se
        Std. Err. adj.:  None
                    CI:  [_meta_cil, _meta_ciu]
              CI level:  95%

      Model and method
                 Model:  Random-effects
                Method:  REML

```python
meta forestplot, nullrefline
graph display
```

      Effect-size label:  Mean Diff.
            Effect size:  _meta_es
              Std. Err.:  _meta_se
            Study label:  id

<img src="./assets/images/05_ContinuousData_14_1.svg"  width="1200" height="800">

## STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis

```python
meta regress year
```

      Effect-size label:  Mean Diff.
            Effect size:  _meta_es
              Std. Err.:  _meta_se

    Random-effects meta-regression                      Number of obs  =        20
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .2539
                                                                  I2 (%) =   58.12
                                                                      H2 =    2.39
                                                           R-squared (%) =    0.00
                                                        Wald chi2(1)   =      0.00
                                                        Prob > chi2    =    0.9729
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            year |  -.0008941   .0262719    -0.03   0.973    -.0523862    .0505979
           _cons |   1.871691   52.49334     0.04   0.972    -101.0134    104.7568
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(18) = 50.78   Prob > Q_res = 0.0001

## STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS

```python
meta funnelplot, metric(invse)
graph di
```

      Effect-size label:  Mean Diff.
            Effect size:  _meta_es
              Std. Err.:  _meta_se
                  Model:  Common-effect
                 Method:  Inverse-variance

<img src="./assets/images/05_ContinuousData_18_1.svg"  width="1200" height="800">

```python
meta bias, egger
```

      Effect-size label:  Mean Diff.
            Effect size:  _meta_es
              Std. Err.:  _meta_se

    Regression-based Egger test for small-study effects
    Random-effects model
    Method: REML

    H0: beta1 = 0; no small-study effects
                beta1 =      0.36
          SE of beta1 =     0.894
                    z =      0.40
           Prob > |z| =    0.6898

```python
meta trimfill
```

      Effect-size label:  Mean Diff.
            Effect size:  _meta_es
              Std. Err.:  _meta_se

    Nonparametric trim-and-fill analysis of publication bias
    Linear estimator, imputing on the left

    Iteration                            Number of studies =     20
      Model: Random-effects                       observed =     20
     Method: REML                                  imputed =      0

    Pooling
      Model: Random-effects
     Method: REML

    ---------------------------------------------------------------
                 Studies |       Mean Diff.    [95% Conf. Interval]
    ---------------------+-----------------------------------------
                Observed |            0.084      -0.214       0.382
      Observed + Imputed |            0.084      -0.214       0.382
    ---------------------------------------------------------------

# CONTINUOUS DATA

STEPS in conducting **Continuous Data**

1. STEP 1 - LOAD DATA
2. STEP 2 - DECLARE, UPDATE & DESCRIBE meta data
3. STEP 3 - SUMMARIZE meta data by using a TABLE or a FOREST PLOT
4. STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis
5. STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS
