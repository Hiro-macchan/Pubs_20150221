Logistic regression and Causal Effect Estimation.
========================================================
author: Hiro_macchan
date: 2015/2/21
at: Tokyo.R #46
css: R_presentation.css

Today I'll talk about 
=====================================================
- Categorical Outcome and Logistic regression
- Causal Effect estimation and Confounders
- Collapsibility and Estimand 
こんなテーマでお話しします。


Who am I?
======================================================
- Matsui Hiroki (RPT, MPH) 
- Major in  
  Rehabilitation Medicine, Clinical Epidemiology and Health Economics.
- Working at  
  The University of Tokyo.  
- Interested in  
  Outcome Research for Health services

Agenda
=======================================================
- **Categorical Outcome**
- <font color = "gray">Logistic regression</font>
- <font color = "gray">Causal Effect estimation</font>
- <font color = "gray">Confounders</font>
- <font color = "gray">Collapsibility</font>
- <font color = "gray">Estimand</font>

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
- <font color = "gray">Categorical Outcome</font>
- **Logistic regression**
- <font color = "gray">Causal Effect estimation</font>
- <font color = "gray">Confounders</font>
- <font color = "gray">Collapsibility</font>
- <font color = "gray">Estimand</font>


Using OLS for Categorical Outcome
========================================================
class: small-code
**multivariate regression (Ordinary Least Square)**
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
-0.70567 -0.17026  0.00602  0.17163  0.61661 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 0.510717   0.008051   63.43   <2e-16 ***
x_1         0.073448   0.001373   53.50   <2e-16 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.2545 on 998 degrees of freedom
Multiple R-squared:  0.7414,	Adjusted R-squared:  0.7412 
F-statistic:  2862 on 1 and 998 DF,  p-value: < 2.2e-16
```

Using multivariate regression model for Categorical Outcome
==================================
class: small-code

![plot of chunk unnamed-chunk-3](TokyoR_46-figure/unnamed-chunk-3-1.png) 
- Y should take two values; $\{0,1\}$,however, there are other values.

***

![plot of chunk unnamed-chunk-4](TokyoR_46-figure/unnamed-chunk-4-1.png) 
- Residual does not distribute Uniformly.



logistic regression
==================================
class: small-code
- Logistic regression NOT Estimate *Y* BUT *Risk (proportion)*  

![plot of chunk unnamed-chunk-5](TokyoR_46-figure/unnamed-chunk-5-1.png) 


logistic regression
==================================
- Logistic regression NOT Estimate *Y* BUT *Risk (proportion)*  
 - $P=f(z)=\frac{1}{1+e^{-z}}$
 - $f(z)$ ; *Logistic Function*  

***
- Logistic function is very similar with the Risk for X.
![plot of chunk unnamed-chunk-6](TokyoR_46-figure/unnamed-chunk-6-1.png) 

logistic regression
==================================
**Logistic Function & Logistic regression **
- $P=f(z)=\frac{1}{1+e^{-z}}$ has a parameter $z$
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
 - 消費者の特性からサービス脱退を予測したい。

Using logistic regression 
===========================================

## Logistic regression is not <font color="red">only</font> a method for predicting outcome.


Agenda
=======================================================
- <font color = "gray">Categorical Outcome</font>
- <font color = "gray">Logistic regression</font>
- **Causal Effect estimation**
- <font color = "gray">Confounders</font>
- <font color = "gray">Collapsibility</font>
- <font color = "gray">Estimand</font>


Using logistic regression  
===========================================
**Estimate Causal Effect **  

- 独立変数対する従属変数の影響度合を知りたい。  
- 見たいパラメータ: $\beta$  

*例えば*  

 - リウマチ症例において、特定の因子（治療や患者背景）が症状の寛解と関連しているか調べたい。
 - 特定消費者へのキャンペーンが購買行動に与える影響を調べたい。 



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

$exp(beta_3)$;   
性別と年齢を補正したうえでの　Treatment odds ratio 

Deference between Prediction model and Causal effect estimation
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
- <font color = "red">交絡因子(Confounders)</font>を全て共変量に含んで居るか。
- 周辺知識を統合して<font color = "red">Back Door Criteria</font>を満たすモデルを作成する必要がある。


Deeper in Causal effect estimation
=======================================================
## Confounders??  
## Back Door Criteria??

Agenda
=======================================================
- <font color = "gray">Categorical Outcome</font>
- <font color = "gray">Logistic regression</font>
- <font color = "gray">Causal Effect estimation</font>
- **Bias and Confounders**
- <font color = "gray">Collapsibility</font>
- <font color = "gray">Estimand</font>

Causal Effect estimation
=========================================
- Causal Effect
  Counterfactual
  ![](Fig_1.png)

Causal Effect estimation
=========================================
- Donald B. Rubin  
  missing value  
  
  <img src="Rubin.jpg" height="200px" width="300px" />
  
*** 

- <font color="red">Judia Pearl  
  Causal Diagram </font>
  <img src="Pearl.jpg" height="200px" width="300px" />  
<b>Directed Acyclic Graphs (DAG)</b>


Bias and Confounders -Thinking with directed acyclic graphs
=======================================================
class: small-code


```r
#install.packages("dagR")
library(dagR)

dag.dat <-
    dag.init(outcome = NULL, exposure = NULL, covs = c(1),
            arcs = c(0,-1,1,0, 1,-1),
             assocs = c(0,0), xgap = 0.04, ygap = 0.05, len = 0.1,
             x.name = "Hospital admission",
             cov.names = c("Confounder; Patient Age"),
             y.name = "Death"
             )

junk <- dag.draw(dag.dat)
```

***

![plot of chunk unnamed-chunk-8](TokyoR_46-figure/unnamed-chunk-8-1.png) 
http://dagitty.net/dags.html?id=FAu9k


Bias and Confounders -Thinking with directed acyclic graphs
=======================================================
![plot of chunk unnamed-chunk-9](TokyoR_46-figure/unnamed-chunk-9-1.png) 
X -> C -> Y ; Back Door, Open Path
***

![plot of chunk unnamed-chunk-10](TokyoR_46-figure/unnamed-chunk-10-1.png) 
Stratify with C, Multivariable regression include C  
Close Back Door

Bias and Confounders -Thinking with directed acyclic graphs
=======================================================
![plot of chunk unnamed-chunk-11](TokyoR_46-figure/unnamed-chunk-11-1.png) 
Closed Path  
http://dagitty.net/dags.html?id=qKWMS
***
![plot of chunk unnamed-chunk-12](TokyoR_46-figure/unnamed-chunk-12-1.png) 
Mediator  
http://dagitty.net/dags.html?id=MnFsp

Quiz
=======================================================
![plot of chunk unnamed-chunk-13](TokyoR_46-figure/unnamed-chunk-13-1.png) 
1:None  
2:C1  
3:C2  
4:C1,C2  
***
![plot of chunk unnamed-chunk-14](TokyoR_46-figure/unnamed-chunk-14-1.png) 
1:C1  
2:C2  
3:C3  
4:None  

Quiz
=======================================================
![plot of chunk unnamed-chunk-15](TokyoR_46-figure/unnamed-chunk-15-1.png) 
1:None  
http://dagitty.net/dags.html?id=47vz5
***
![plot of chunk unnamed-chunk-16](TokyoR_46-figure/unnamed-chunk-16-1.png) 
4:None  
http://dagitty.net/dags.html


Quiz
=======================================================
![plot of chunk unnamed-chunk-17](TokyoR_46-figure/unnamed-chunk-17-1.png) 

1:None, 2:C1, 3:C2, 4:C1,C2  

Quiz
=======================================================
<img src="TokyoR_46-figure/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" style="display: block; margin: auto;" />

<center>3:C2  
http://dagitty.net/dags.html?id=VFh3B</center>

Deference between Prediction model and Causal effect estimation
========================================================
**Prediction of Outcome**
- 従属変数を所与として、結果が生じる確率を知りたい。
- 見たいパラメータ: $P$ 
- 必要な予測因子を共変量に含んで居るか。
- クロスバリデーションやBootstrap AICなどで変数選択・モデル妥当性の確認が可能
- 予測力の多寡で機械的な共変量選択が可能。

***

**Causal Effect Estimation**  
- 独立変数対する従属変数の影響度合を知りたい。  
- 見たいパラメータ: $\beta$  
- <font color = "red">交絡因子</font>を全て共変量に含んで居るか。
- 周辺知識を統合して<font color = "red">Back Door Criteria</font>を満たすモデルを作成する必要がある。
- <font color = "red">機械的な共変量選択があまり意味をなさない。</font>

Private Opinion
========================================================
# 因果関係推察する分析するときに、予測力の多寡でモデルの良し悪し語るな。

追加的内容
=======================================================
memo: ここから先の内容は時間があったら話す。

Agenda
=======================================================
- <font color = "gray">Categorical Outcome</font>
- <font color = "gray">Logistic regression</font>
- <font color = "gray">Causal Effect estimation</font>
- <font color = "gray">Bias and Confounders</font>
- **Collapsibility**
- <font color = "gray">Estimand</font>


Quiz
=======================================================
<img src="TokyoR_46-figure/unnamed-chunk-19-1.png" title="plot of chunk unnamed-chunk-19" alt="plot of chunk unnamed-chunk-19" style="display: block; margin: auto;" />

<center>1:C1, 2:C2, 3:C1,C2, 4:None</center>


Test
=======================================================
class: small-code

![plot of chunk unnamed-chunk-20](TokyoR_46-figure/unnamed-chunk-20-1.png) 

3:C1,C2 ?  
http://dagitty.net/dags.html?id=g7mzV  
https://rpubs.com/Hiro_macchan/53793  

Collapsibility
========================================================
## Odds Ratio に着目する場合、Predictor を補正しないとOdds Ratio が0方向にバイアスされる。
## Non-collapsibility と呼ばれる性質

Agenda
=======================================================
- <font color = "gray">Categorical Outcome</font>
- <font color = "gray">Logistic regression</font>
- <font color = "gray">Causal Effect estimation</font>
- <font color = "gray">Bias and Confounders</font>
- <font color = "gray">Collapsibility</font>
- **Estimand**

Estimand
=========================================================
## =Estimate + Demand
## 推定したいもの？

## Non-collapsibility を回避するなら、Estimand をOdds Ratio から切り替える。
Average Partial Effect(APE)
============================================================
APE とはロジスティックモデルの結果を利用した仮想的なRisk Differnce にあたる。  
2値変数x のAPEを求める場合、すべての症例がx=1 であった場合の推計イベント割合から、すべての症例がx=0 であった場合の推計イベント割合を引いて算出される。  
具体的には以下の式による[2]。

$g(z) = \frac{1}{1+exp(-z)}$  

$APE = \hat{\beta_K}(N^{-1}\sum_{i=1}^{N}{g(x_i\hat{\beta})}) \cdots x_K がcontinuous$  

$APE = N^{-1}\sum_{i=1}^{N}{[g(\hat{\beta_1}+\hat{\beta_2}x_{i2}+\cdots+\hat{\beta_{K-1}}x_{i,K-1}+\hat{\beta_{K}})-g(\hat{\beta_1}+\hat{\beta_2}x_{i2}+\cdots+\hat{\beta_{K-1}}x_{i,K-1})]} \cdots x_K がbinary$  

Average Partial Effect(APE)
============================================================
![plot of chunk unnamed-chunk-21](TokyoR_46-figure/unnamed-chunk-21-1.png) 


Private Opinion
========================================================
# 因果関係推察する分析するときに、予測力の多寡でモデルの良し悪し語るな。
# 因果の大きさを見るときには、Estimand に気を付けろ。

Reference
========================================================
- Rothman, K. J., et al. (2008). Modern epidemiology, Wolters Kluwer Health   : Lippincott Williams & Wilkins.
- Wooldridge, J. M. (2010). Econometric Analysis of Cross Section and Panel   Data, MIT Press.
- Pearl, J. and 学. 黒木 (2009). 統計的因果推論 : モデル・推論・推測, 共立出版.
- 宮川, 雅. (2004). 統計的因果推論 : 回帰分析の新しい枠組み, 朝倉書店.
- http://rpubs.com/kaz_yos/
- http://www.slideshare.net/takehikoihayashi/ss-13441401
- https://healthpolicyhealthecon.wordpress.com/2014/12/12/causal-diagram/
