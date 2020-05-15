---
layout: page
title: Binary meta
# permalink: /binary/
---

# BINARY DATA

## Learning outcomes:

You will have _knowledge_ about and be able:

- Conduct fixed and random effects meta-analysis
- Measure effect size
- Explore heterogeneity using sub-group and meta-regression analyses
- Explore and address small study effects using funnel plot and trim and fill

STEPS in conducting **Binary Data** meta-analysis, i.e. mortality (yes or no), hypertensive (yes or no)

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

<iframe width="750" height="480"
  src="https://www.youtube.com/embed/6nBPKENPDAs" 
  frameborder="2" 
  allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" 
  allowfullscreen>
</iframe>
<!-- <a href="http://www.youtube.com/watch?feature=player_embedded&v=6nBPKENPDAs" target="_blank"><img src="http://img.youtube.com/vi/6nBPKENPDAs/0.jpg" alt="03-Binary MetaAnalysis" width="1200" height="800" border="10" /></a> -->

## STEP 1 - LOAD DATA

Data from a set of trials is provided called ‘magnes.dta’. 16 RCTs report results that assessed the impact of IV magnesium in patients with a suspected acute myocardial infarction (MI). The outcome was all cause death. The question is whether the use of IV magnesium post-MI improves survival?

This data set was associated with controversy. A meta-analysis published in BMJ 1991 (303; 1499-1503) concluded magnesium is.”....an effective safe, simple.” .intervention. But a mega-trial published in 1995 concluded the intervention was useless. Thus a conflict between results of meta-analysis of several small trials and a powerful mega-trial led to worries about the reliability of meta-analyses and questions concerning the possible reasons for the observed discrepancy.

| **trial** | **trialnam** | **year** | **tot1** | **dead1** | **tot0** | **dead0** |
| --------- | ------------ | -------- | -------- | --------- | -------- | --------- |
| 1         | Morton       | 1984     | 40       | 1         | 36       | 2         |
| 2         | Rasmussen    | 1986     | 135      | 9         | 135      | 23        |
| 3         | Smith        | 1986     | 200      | 2         | 200      | 7         |

...

**Option 1**
Copy and paste from Excel (see attached magnedata.xlsx below)

---

`edit`

**Option 2**
Input raw directly into Stata (using ‘do file editor’ type `doedit`)

---

`clear all input trial str20 trialnam year dead1 dead0 tot1 tot0 1 Morton 1984 1 2 40 36 2 Rasmussen 1986 9 23 135 135 3 Smith 1986 2 7 200 200 4 Abraham 1987 1 1 48 46 5 Feldstedt 1988 10 8 150 148 6 Schechter 1989 1 9 59 56 7 Ceremuzynski 1989 1 3 25 23 8 Bertschat 1989 0 1 22 21 9 Singh 1990 6 11 76 75 10 Pereira 1990 1 7 27 27 11 Schechter 1 1991 2 12 89 80 12 Golf 1991 5 13 23 33 13 Thogersen 1991 4 8 130 122 14 LIMIT-2 1992 90 118 1159 1157 15 Schechter 2 1995 4 17 107 108 16 ISIS-4 1995 2216 2103 29011 29039 end`

**Option 3**
Load Stata data file directly from Excel file

`import excel using data/magnedata.xlsx, firstrow`

---

**Option 4**
Load Stata data file directly (if already saved)

---

```python
use data/magnedata.dta, clear
describe
```

    Contains data from data/magnedata.dta
      obs:            16
     vars:             7                          11 Oct 2015 21:16
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    trial           byte    %8.0g
    trialnam        str12   %12s
    year            int     %8.0g
    dead1           int     %8.0g
    dead0           int     %8.0g
    tot1            int     %8.0g
    tot0            int     %8.0g
    --------------------------------------------------------------------------------
    Sorted by:

**Data presentation**

The syntax is as follows: the command name followed by four variable names. The variables, in order, should represent the number of events and non-events in the experimental group and the number of events and non-events in the control group.

Note that our data set does not contain variables representing the number of non-events. We must generate these variables ourselves. The following should suffice:

```python
generate nodead1 = tot1 - dead1
generate nodead0 = tot0 - dead0
order trial trialnam year dead1 nodead1 dead0 nodead0
```

## STEP 2- DECLARE, UPDATE & DESCRIBE meta data

| Binary type | Description                                                        |
| ----------- | ------------------------------------------------------------------ |
| lnoratio    | log odds-ratio; the default                                        |
| lnrratio    | log risk-ratio (also known as log rate ratio and log relative risk |
| rdiff       | risk difference                                                    |
| lnorpeto    | Peto's log odds-ratio                                              |

```python
meta esize dead1 nodead1 dead0 nodead0, studylabel(trialnam) esize(lnrratio)
```

    Meta-analysis setting information

     Study information
        No. of studies:  16
           Study label:  trialnam
            Study size:  _meta_studysize
          Summary data:  dead1 nodead1 dead0 nodead0

           Effect size
                  Type:  lnrratio
                 Label:  Log Risk-Ratio
              Variable:  _meta_es
       Zero-cells adj.:  0.5, only0

             Precision
             Std. Err.:  _meta_se
                    CI:  [_meta_cil, _meta_ciu]
              CI level:  95%

      Model and method
                 Model:  Random-effects
                Method:  REML

## STEP 3- SUMMARIZE meta data by using a TABLE or a FOREST PLOT

> _Now, combine the results of trials, using the **fixed effects model** for crude data to calculate a summary estimate_

> _Q1.1 - From a visual inspection of the forest plot generated by Stata, do the measures of outcome appear consistent across studies (just provide a qualitative/descriptive answer based on the data)?_

<details><summary>Click For Answer</summary>
<p>

Not consistent, the RR ranged 0.11 (in Schechter) to 1.23 (in Feldstedt)

</p>
</details>

> _Q1.2 - What is the summary estimate and 95% confidence interval?_

<details><summary>Click For Answer</summary>
<p>

Pooled RR=1.01; 95% CI 0.95, 1.06

</p>
</details>

> _Q1.3 - Which study is weighted the most heavily, and how does the point estimate for this study compare to the overall summary estimate?_

<details><summary>Click For Answer</summary>
<p>

ISIS-4 (89.8%)

</p>
</details>

> _Q1.4 - How would you interpret the results?_

<details><summary>Click For Answer</summary>
<p>

There is no statistically significance difference in mortality rate between those treated with IV Mg and those not treated with IV Mg.

</p>
</details>

---

```python
meta summarize, fixed
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se
            Study label:  trialnam

    Meta-analysis summary                     Number of studies =     16
    Fixed-effects model                       Heterogeneity:
    Method: Mantel-Haenszel                             I2 (%) =   66.79
                                                            H2 =    3.01

    --------------------------------------------------------------------
                Study | Log Risk-Ratio    [95% Conf. Interval]  % Weight
    ------------------+-------------------------------------------------
               Morton |         -0.799      -3.156       1.559      0.09
            Rasmussen |         -0.938      -1.671      -0.206      0.98
                Smith |         -1.253      -2.812       0.306      0.30
              Abraham |         -0.043      -2.785       2.700      0.04
            Feldstedt |          0.210      -0.692       1.111      0.34
            Schechter |         -2.249      -4.283      -0.216      0.39
         Ceremuzynski |         -1.182      -3.373       1.009      0.13
            Bertschat |         -1.143      -4.290       2.004      0.07
                Singh |         -0.619      -1.562       0.323      0.47
              Pereira |         -1.946      -3.972       0.080      0.30
          Schechter 1 |         -1.898      -3.365      -0.432      0.54
                 Golf |         -0.594      -1.478       0.289      0.46
            Thogersen |         -0.757      -1.931       0.418      0.35
              LIMIT-2 |         -0.273      -0.535      -0.011      5.04
          Schechter 2 |         -1.438      -2.493      -0.382      0.72
               ISIS-4 |          0.053      -0.004       0.111     89.76
    ------------------+-------------------------------------------------
                theta |          0.006      -0.049       0.061
    --------------------------------------------------------------------
    Test of theta = 0: z = 0.20                      Prob > |z| = 0.8414
    Test of homogeneity: Q = chi2(15) = 45.17          Prob > Q = 0.0001

```python
meta forestplot, fixed
graph display
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se
            Study label:  trialnam

<img src="./assets/images/03_BinaryData_14_1.svg"  width="1200" height="800">

---

_Stata gives you many options to customize the Forest plot and the summary table._

---

```python
meta forestplot, nullrefline fixed eform
graph display
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se
            Study label:  trialnam

<img src="./assets/images/03_BinaryData_16_1.svg"  width="1200" height="800">

---

_OR combined the results using **Random effects model**._

> _Q1.5 - Do the summary estimate and 95% confidence interval differ from the fixed effects model?_

<details><summary>Click For Answer</summary>
<p>

- Fixed: 1.01 , 95% CI 0.95 to 1.06
- Random: 0.51, 95% CI 0.35 to 0.74

</p>
</details>

> _Q1.6 - Compare the relative weights of the studies in the fixed effects model with their weights in the random effects model. Discuss how the relative weights affect the summary estimates and 95% confidence intervals._

<details><summary>Click For Answer</summary>
<p>

- Fixed: 90%
- Random: 16%

</p>
</details>

---

```python
meta forestplot, nullrefline random eform
graph di
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se
            Study label:  trialnam

<img src="./assets/images/03_BinaryData_18_1.svg"  width="1200" height="800">

```python
describe
```

    Contains data from data/magnedata.dta
      obs:            16
     vars:            17                          11 Oct 2015 21:16
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    trial           byte    %8.0g
    trialnam        str12   %12s
    year            int     %8.0g
    dead1           int     %8.0g
    nodead1         float   %9.0g
    dead0           int     %8.0g
    nodead0         float   %9.0g
    tot1            int     %8.0g
    tot0            int     %8.0g
    _meta_id        byte    %9.0g                 Study ID
    _meta_studyla~l str12   %12s                  Study label
    _meta_es        double  %10.0g                Log risk-ratio
    _meta_se        double  %10.0g                Std. Err. for log risk-ratio
    _meta_cil       double  %10.0g                95% lower CI limit for log
                                                    risk-ratio
    _meta_ciu       double  %10.0g                95% upper CI limit for log
                                                    risk-ratio
    _meta_studysize float   %9.0g                 Sample size per study
    _meta_weight    double  %10.0g                M-H weights for log risk-ratio
    --------------------------------------------------------------------------------
    Sorted by:
         Note: Dataset has changed since last saved.

---

In Stata, Q is labelled “Heterogeneity chi-squared.”

> _Q1.7 - Are the results homogeneous?_

<details><summary>Click For Answer</summary>
<p>

No, there is statistically significant moderate (_I<sup>2</sup>_=67%, p<0.000)

[ low (0-40%), moderate (30-60%), substantial (50-90%), considerable (75-100%). Though depends on (i) magnitude and direction of effects and (ii) strength of evidence for heterogeneity (e.g. P value from the chi-squared test, or a confidence interval for _I<sup>2</sup>_)

</p>
</details>

> _Q1.8 - What is heterogeneity and what factors can account for heterogeneity?_

<details><summary>Click For Answer</summary>
<p>

the variation in study outcomes between studies: clinical, methodological or statistical

</p>
</details>

> _Q1.9 - Which trial(s) do you suspect contributes the most to heterogeneity in this analysis? Why?_

<details><summary>Click For Answer</summary>
<p>

ISI-4, sample size

</p>
</details>

---

```python

```

## STEP 4- EXPLORE HETEROGENEITY - SUB-GROUP and META-REGRESSION analysis

Perform a subgroup analysis to examine the effect of study sample sizes on the pooled estimates. To do this, calculate the pooled estimates for studies with smaller sample sizes (<1000) and those with larger sample sizes (≥1000).

You will want to create a variable for sample size group. The following will work:

```python
recode _meta_studysize (min/999 = 0 "Smaller RCTs") (1000/max = 1 "Mega-RCTs"), gen(size)
```

    (16 differences between _meta_studysize and size)

---

The resulting variable (size) is coded 0 if the sample size is less than 1000 and 1 if the sample is larger than 1000.

Re-do the analyses for the smaller- and mega-RCTs groups, and compare the summary estimates for each group (using by `subgroup` in meta).

---

---

> _Q1.10 - Does exclusion of this trial(s) “fix” the heterogeneity?_

<details><summary>Click For Answer</summary>
<p>

Yes

</p>
</details>

> _Q1.11 - What are the summary estimates for each group?_

<details><summary>Click For Answer</summary>
<p>

- Smaller-RCTs (RR=0.39,95% CI 0.29, 0.54)
- Meta-RCTs (RR=1.04; 95% CI 0.98, 1.10)

</p>
</details>

> _Q1.12 - Are the results homogeneous for each group?_

<details><summary>Click For Answer</summary>
<p>

- Smaller-RCTs (Homog., _I<sup>2</sup>_=0%, p=.448)
- Mega-RCTs (Heterog., _I<sup>2</sup>_=82%, p=.017)

</p>
</details>

---

```python
meta forestplot, subgroup(size)  nullrefline fixed eform
graph display
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se
            Study label:  trialnam

<img src="./assets/images/03_BinaryData_26_1.svg"  width="1200" height="800">

---

_Fit a meta-regression model that explains the heterogeneity in terms of the study size category_

> _Q1.13 - Is the study size statistically significant?_

<details><summary>Click For Answer</summary>
<p>

Yes, `p = 0.002`

</p>
</details>

> _Q1.14 - What percentage of heterogeneity is explained by the study size_

<details><summary>Click For Answer</summary>
<p>

The proportion of between-study variance explained by the covariates can be assessed via the R<sup>2</sup> statistic. Here roughly **78%** of the between-study variance is explained by the covariate study size.

</p>
</details>

---

```python
meta regress size
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se

    Random-effects meta-regression                      Number of obs  =        16
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .0501
                                                                  I2 (%) =   33.01
                                                                      H2 =    1.49
                                                           R-squared (%) =   77.92
                                                        Wald chi2(1)   =      9.66
                                                        Prob > chi2    =    0.0019
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            size |   .7823604   .2517851     3.11   0.002     .2888707     1.27585
           _cons |   -.868714   .1851311    -4.69   0.000    -1.231564   -.5058636
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(14) = 18.42   Prob > Q_res = 0.1882

```python
meta regress year
estat bubbleplot
graph display
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se

    Random-effects meta-regression                      Number of obs  =        16
    Method: REML                                        Residual heterogeneity:
                                                                    tau2 =   .1965
                                                                  I2 (%) =   56.42
                                                                      H2 =    2.29
                                                           R-squared (%) =   13.38
                                                        Wald chi2(1)   =      0.79
                                                        Prob > chi2    =    0.3730
    ------------------------------------------------------------------------------
        _meta_es |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            year |   .0522546   .0586504     0.89   0.373     -.062698    .1672072
           _cons |  -104.6763   116.7574    -0.90   0.370    -333.5166     124.164
    ------------------------------------------------------------------------------
    Test of residual homogeneity: Q_res = chi2(14) = 21.78   Prob > Q_res = 0.0833

<img src="./assets/images/03_BinaryData_29_1.svg"  width="1200" height="800">

---

**Cumulative meta-analysis**

_Explore how the pooled result would have changed through time by using the cumulative meta-analysis command metacum_

> _Q1.15 - What do you conclude?_

<details><summary>Click For Answer</summary>
<p>

It became evidence that IV Mg, was effective as of 1998

</p>
</details>

---

```python
meta forestplot, cumulative(year) nullrefline
graph display
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se
            Study label:  trialnam

<img src="./assets/images/03_BinaryData_31_1.svg"  width="1200" height="800">

## STEP 5- EXPLORE and ADDRESS SMALL-STUDY EFFECTS

_Explore the possibility that the data set is subject to “small study” (or “publication”) bias_

For example, you can produce a funnel plot using the meta funnelplot command. Note that the standard error of the effect size should be proportional to the sample size, because the smaller the study is the larger the variance.

Publication bias arises when smaller studies with nonsigniﬁcant ﬁndings are being suppressed from publication. It is one of the more common reasons for the presence of small-study effects, which leads to the asymmetry of the funnel plot. Another common reason for the asymmetry in the funnel plot is the presence of between-study heterogeneity.

---

> _Q1.16 - Does the funnel plot show evidence of publication bias?_

<details><summary>Click For Answer</summary>
<p>

Yes, missing studies (right side)

</p>
</details>

---

```python
meta funnelplot
graph display
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se
                  Model:  Common-effect
                 Method:  Inverse-variance

<img src="./assets/images/03_BinaryData_33_1.svg"  width="1200" height="800">

---

- On a funnel plot, the more precise trials (with smaller standard errors) are displayed at the top of the funnel, and the less precise ones (with larger standard errors) are displayed at the bottom.

- The red reference line is plotted at the estimate of the overall effect size, the overall log odds-ratio in our example.

- In the absence of small-study effects, we would expect the points to be scattered around the reference line with the effect sizes from smaller studies varying more around the line than those from larger studies, forming the shape of an inverted funnel.

- In our plot, there is an empty space in the bottom right corner. This suggests that the smaller trials with log risk-ratio estimates close to zero may be missing from the meta-analysis.

---

---

- The asymmetry is evident in the funnel plot from above, but we do not know the cause for this asymmetry. The asymmetry can be the result of publication bias or may be because of other reasons.

- The so-called contour-enhanced funnel plots can help determine whether the asymmetry of the funnel plot is because of publication bias.

- The contour lines that correspond to certain levels of statistical signiﬁcance (1%, 5%, and 10%) of tests of individual effects are overlaid on the funnel plot. Generally, publication bias is suspect when smaller studies are missing in the nonsigniﬁcant regions.

* _Let’s add the 1%, 5%, and 10% signiﬁcance contours to our funnel plot by specifying them in the contours() option._

---

```python
meta funnelplot, contours(1 5 10)
graph display
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se
                  Model:  Common-effect
                 Method:  Inverse-variance

<img src="./assets/images/03_BinaryData_36_1.svg"  width="1200" height="800">

---

**Testing for small-study effects**

We can test for the presence of small-study effects or, technically, the asymmetry in the funnel plot more formally by using, for example, one of the regression-based tests. The main idea behind these tests is to determine whether there is a statistically signiﬁcant association between the effect sizes and their measures of precision such as effect-size standard errors.

Use the metabias command to perform statistical tests for publication bias, a regression asymmetry test (Egger’s test)

> _Q1.17 - Do the statistical tests suggest evidence of publication bias?_

<details><summary>Click For Answer</summary>
<p>

Yes, there is evidence of publication bias, p-value for small-study effects `p = 0.000`

</p>
</details>

---

```python
meta bias, egger
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se

    Regression-based Egger test for small-study effects
    Random-effects model
    Method: REML

    H0: beta1 = 0; no small-study effects
                beta1 =     -1.45
          SE of beta1 =     0.317
                    z =     -4.59
           Prob > |z| =    0.0000

---

**Trim-and-ﬁll analysis for addressing publication bias**

- When the presence of publication bias is suspected, it is important to explore its impact on the ﬁnal meta-analysis results. The trim-and-ﬁll method of Duval and Tweedie provides a way to evaluate the impact of publication bias on the results.

- The idea of the method is to estimate the number of studies potentially missing because of publication bias, impute these studies, and use the observed and imputed studies to obtain the overall estimate of the effect size. \*

- This estimate can then be compared with the estimate obtained using only the observed studies.

* We suspect the presence of publication bias in the meta-analysis above. Let’s use the trim-and-ﬁll method to investigate the impact of potentially missing studies on the estimate of the overall log risk-ratio.

> _Q1.18 - Examine the graph. How many filled studies?_

<details><summary>Click For Answer</summary>
<p>

meta trimfill reports that 7 hypothetical studies are estimated to be missing.

</p>
</details>

> _Q1.19 - What are the new pooled RR estimates?_

<details><summary>Click For Answer</summary>
<p>

When 7 studies are imputed and added to the meta-analysis, the treatment effect is no longer statistically significant after adding the hypothetical missing studies `[(0.51 (0.35 to 0.74) versus 0.71 (95 0.48 to 1.04)]`. This suggests that the treatment beneﬁt as reported in the literature may be smaller than it would be in the absence of publication bias.

From the funnel plot, almost all the imputed studies fall in the darkest-gray region corresponding to a p-value of more than 10%. This further supports the conclusion that the small-study effect is most likely because of publication bias.

</p>
</details>

For viewing the filled studies in a funnel plot:

---

```python
meta trimfill, eform funnel(contours(1 5 10))
graph display
```

      Effect-size label:  Log Risk-Ratio
            Effect size:  _meta_es
              Std. Err.:  _meta_se

    Nonparametric trim-and-fill analysis of publication bias
    Linear estimator, imputing on the right

    Iteration                            Number of studies =     23
      Model: Random-effects                       observed =     16
     Method: REML                                  imputed =      7

    Pooling
      Model: Random-effects
     Method: REML

    ---------------------------------------------------------------
                 Studies |       Risk Ratio    [95% Conf. Interval]
    ---------------------+-----------------------------------------
                Observed |            0.511       0.352       0.742
      Observed + Imputed |            0.709       0.485       1.037
    ---------------------------------------------------------------

<img src="./assets/images/03_BinaryData_40_1.svg"  width="1200" height="800">

## Sections

1. [Overview meta-analysis data types](index.markdown)
2. [Introduction to Stata](b_stata.markdown)
3. [Meta-analysis of binary data](c_binary.markdown)
4. [Meta-analysis of generic data](d_generic.markdown)
5. [Meta-analysis of continuous data](e_continuous.markdown)
6. [Meta-analysis of prevalence data](f_prevalence.markdown)
