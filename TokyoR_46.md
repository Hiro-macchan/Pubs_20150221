Logistic regression Tokyo.R #46
========================================================
author: Hiro_macchan
date: 2015/2/21  
css: R_presentation.css

Today I'll talk about 
=====================================================
- Categorical Outcome and Logistic regression
- Causal Effect estimation and Confounders
- Estimand and Colapsibility



Who am I?
======================================================
- Matsui Hiroki(RPT, MPH) 
- Major in  
  Rehabilitation Medicine, Clinical Epidemiology and Health Economics.
- Working at  
  The University of Tokyo.  
- Interested in  
  Outcome Research for Health services
- I'm not so familiar with English.  
  So, if you have any questions, please interrupt and ask me. 

Agenda
=======================================================
- **Categorical Outcome**
- <font color = "gray">Logistic regression</font>
- <font color = "gray">Causal Effect estimation</font>
- <font color = "gray">Confounders</font>
- <font color = "gray">Estimand</font>
- <font color = "gray">Colapsibility</font>

Categorical Outcome
========================================================
- Dichotomous (or multiple choice) variable Outcome
- Familiar with
 - Death (1 = death)
 - Readmission (1 = readmission)
 - mRS (ADL Score, 1~5 category)
- In Other fields
 - Registration (1 = registration)
 - Withdrawal (1 = Withdrawal)

Agenda
=======================================================
- Categorical Outcome
- *Logistic regression*
- Causal Effect estimation
- Confounders
- Estimand
- Colapsibility


Using OLS for Categorical Outcome
========================================================
class: small-code
**multivariate regression(Ordinaly Least Square)**
$$
Y = \beta_0 + \beta_1x_1 + \eta  
$$
$$
Y = \{y|0,1\}
$$




```r
summary(lm(y~x_1))
```

```

Call:
lm(formula = y ~ x_1)

Residuals:
     Min       1Q   Median       3Q      Max 
-0.63546 -0.17950  0.00148  0.18687  0.63599 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 0.486928   0.008292   58.73   <2e-16 ***
x_1         0.072763   0.001416   51.37   <2e-16 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.2622 on 998 degrees of freedom
Multiple R-squared:  0.7256,	Adjusted R-squared:  0.7253 
F-statistic:  2639 on 1 and 998 DF,  p-value: < 2.2e-16
```

Using multivariate regression model for Categorical Outcome
==================================
class: small-code

![plot of chunk unnamed-chunk-3](TokyoR_46-figure/unnamed-chunk-3-1.png) 
- Y should take two values; $\{0,1\}$,however, there are other values.

***

![plot of chunk unnamed-chunk-4](TokyoR_46-figure/unnamed-chunk-4-1.png) 
- Residual are not Uniformly distribution.



logistic regression
==================================
class: small-code
- Logistic regression Not Estimate *Y* But *Risk (propotion)*  

![plot of chunk unnamed-chunk-5](TokyoR_46-figure/unnamed-chunk-5-1.png) 


logistic regression
==================================
- Logistic regression Not Estimate *Y* But *Risk (propotion)*  
 - $P=f(z)=\frac{1}{1+e^{-z}}$
 - $f(z)$ ; *Logistic Function*  

***
- Logistic function is very similar with the Risk for X.
![plot of chunk unnamed-chunk-6](TokyoR_46-figure/unnamed-chunk-6-1.png) 

logistic regression
==================================
**Logistic Function & Logistic regression **
- $P=f(z)=\frac{1}{1+e^{-z}}$ has a palameter $z$
- たとえば年齢が高いとリスクが高いという状況は、「$z$はAGEが高いほど大きくなる」とあらわせる。  
  $z = \beta_0+\beta_1x_1 + \beta_2x_2 + \cdots+\beta_kx_k$  
  $f(z) = \frac{1}{1+e^{-z}}$  
  $f(z) = \frac{1}{1+e^{-\beta_0+\beta_1x_1 + \beta_2x_2 + \cdots+\beta_kx_k}}$
- あらわされる関数($f(z)$)が実際のデータから算出されるPをうまくあらわすように$\beta_0 \cdots \beta_kx_k$ を設定してあげる。
- 設定の仕方（最尤推定）はソフトウェアに任せる。
- 一般線形モデルを一般化した線形モデルなので**一般化線形モデル**などと呼ばれる


Using logistic regression 
===========================================
**Prediction of Outcome**
- 従属変数を所与として、それぞれの症例のアウトカムが生じる確率を知りたい。
- 見たいパラメータ: $P$  

*e.g.*  
 - リウマチ症例において、症状が寛解するかどうかを各種予後因子から予測したい。
 - Predict withdrowal probability from costomers characteristics.

Using logistic regression 
===========================================

## Logistic regression is not <font color="red">only</font> a method for predicting outcome.


Agenda
=======================================================
- <font color = "gray">Categorical Outcome</font>
- <font color = "gray">Logistic regression</font>
- **Causal Effect estimation**
- <font color = "gray">Confounders</font>
- <font color = "gray">Estimand</font>
- <font color = "gray">Colapsibility</font>

Causal Effect estimation
=========================================
- Causal Effect
  Counterfactual



Using logistic regression 
===========================================
**Estimate Causal Effect **  

- 独立変数対する従属変数の影響度合を知りたい。  
- 見たいパラメータ: $\beta$  

*例えば*  

 - リウマチ症例において、特定の因子（治療や患者背景）が症状の寛解と関連しているか調べたい。
 - Estimate the effect from Costomer Specific campaign. 



Odds Ratio
==================================
**Logistic regression とOdds Ratio**
- $P=f(z)=\frac{1}{1+e^{-z}}$をzについて解く
$z = log(\frac{p}{1-p})$
- $\frac{p}{1-p}$ はOddsをあらわす。
- つまり、男性のz を$z_m$ 女性のz を$z_f$とすると、$z = \beta_0+\beta_1Sex + \beta_2Age$ の$\beta_1$が示すものは$log(OR)$となる。

*** 

$\beta_1 = \frac{z_m-z_f}{1-0}$  
$=log(\frac{p_m}{1-p_m})-log(\frac{p_f}{1-p_f})$  
$= log(\frac{p_m}{1-p_m}/\frac{p_f}{1-p_f})$  
$OR = \frac{p_m}{1-p_m}/\frac{p_f}{1-p_f}= e^{\beta_1}$


年齢（連続変数）の場合年齢が1単位増加した場合のオッズ比を算出できる。  
$\beta$の信頼区間の出し方などはソフトウェアに任せましょう。

Odds Ratio
========================================================
**割合の比較**

    x   | col1   | col2
  ----|--------|--------
  row1| a     | b
  row2| c      | d
  - リスク  
    $Risk = \frac{a}{a+b}$
  - オッズ  
    $Odds = \frac{a/a+b}{b/a+b} = \frac{a}{b}$

***
 - リスク差
    $$
     \hat{RD} = \frac{c}{c+d} 
    $$
 - リスク比
    $$
    \hat{RR} = \frac{a}{a+b} / \frac{c}{c+d}
    $$
 - オッズ比
    $$
    \hat{OR} = \frac{a}{b} / \frac{c}{d}
    $$

Adjusted Odds Ratio
========================================================
$$
z = logit(p) =log(\frac{p}{1-p}) = \beta_0+\beta_1Sex + \beta_2Age + \beta_3Treat
$$

$exp(beta_3)$; Treatment odds ratio adjusted for patient age and sex

Defference between Prediction model and Causal effect estimation
========================================================
**Prediction of Outcome**
- 従属変数を所与として、結果が生じる確率を知りたい。
- 見たいパラメータ: $P$ 
- 必要な予測因子を共変量に含んで居るか。
- クロスバリデーションやBootstrap AICなどで変数選択・モデル妥当性の確認が可能

***

**Causal Effect Estimation**  
- 独立変数対する従属変数の影響度合を知りたい。  
- 見たいパラメータ: $\beta$  
- <font color = "red">交絡因子</font>を全て共変量に含んで居るか。
- 周辺知識を統合して<font color = "red">Back Door 基準</font>を満たすモデルを作成する必要がある。

Agenda
=======================================================
- <font color = "gray">Categorical Outcome</font>
- <font color = "gray">Logistic regression</font>
- <font color = "gray">Causal Effect estimation</font>
- **Bias and Confounders**
- <font color = "gray">Estimand</font>
- <font color = "gray">Colapsibility</font>

Bias and Confounders
=======================================================
class: small-code

```r
#install.packages("dagR")
library(dagR)

dag.dat <-
    dag.init(outcome = NULL, exposure = NULL, covs = c(1),
             arcs = c(1,0, 1,-1),
             assocs = c(0,0), xgap = 0.04, ygap = 0.05, len = 0.1,
             x.name = "Preterm birth",
             cov.names = c("Maternal Age"),
             y.name = "Later Life Maternal CVD"
             )

junk <- dag.draw(dag.dat, noxy = T)
```

![plot of chunk unnamed-chunk-7](TokyoR_46-figure/unnamed-chunk-7-1.png) 

```r
dag.draw(demo.dag1())
```

![plot of chunk unnamed-chunk-7](TokyoR_46-figure/unnamed-chunk-7-2.png) 

```
$cov.types
[1]  0  1  1  1 -1

$x
[1] 0.000000000 0.005025253 0.505000000 0.994974747 1.000000000

$y
[1] 0.0000000 0.4949747 0.2690000 0.4949747 0.0000000

$arc
     [,1] [,2]
[1,]    2    1
[2,]    2    3
[3,]    4    3
[4,]    4    5

$arc.type
[1] 0 0 0 0

$curve.x
[1] NA NA NA NA

$curve.y
[1] NA NA NA NA

$xgap
[1] 0.04

$ygap
[1] 0.05

$len
[1] 0.1

$names
[1] "neighborhood violence" "urban residence"       "participation"        
[4] "family history"        "incident CVD"         

$symbols
[1] NA NA NA NA NA

$version
[1] "1.1.3"

attr(,"class")
[1] "dagRdag"
```

