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
- *Categorical Outcome*
- Logistic regression
- Causal Effect estimation
- Confounders
- Estimand
- Colapsibility

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
-0.73904 -0.18275 -0.00622  0.18720  0.70981 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 0.494942   0.008207   60.31   <2e-16 ***
x_1         0.074214   0.001424   52.12   <2e-16 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.2594 on 998 degrees of freedom
Multiple R-squared:  0.7313,	Adjusted R-squared:  0.731 
F-statistic:  2716 on 1 and 998 DF,  p-value: < 2.2e-16
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

# Logistic regression is not only used in prediction fields.


Agenda
=======================================================
- <font color = "gray">Categorical Outcome</font>
- <font color = "gray">Logistic regression</font>
- **Causal Effect estimation**
- <font color = "gray">Confounders</font>
- <font color = "gray">Estimand</font>
- <font color = "gray">Colapsibility</font>


Using logistic regression 
===========================================
**Estimate Causal Effect **  

- 独立変数対する従属変数の影響度合を知りたい。  
- 見たいパラメータ: $\beta$  

*例えば*  

 - リウマチ症例において、特定の因子（治療や患者背景）が症状の寛解と関連しているか調べたい。
 - Estimate the effect from Costomer Specific campaign. 



logistic regression
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
