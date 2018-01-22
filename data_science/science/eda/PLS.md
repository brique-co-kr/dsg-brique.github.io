---
title: "PLS(Partial Least Square)"
author: "Sherry Jeon, Arina Choi"
date: "2018년 1월 15일"
output: html_document
---
<br/>
<br/>

## 1. Multiple Linear Regression (다중 선형 회귀 분석)  
  * 정의 : 두 개 이상의 독립변수(X)들과 하나의 종속변수(Y)의 관계를 분석하는 기법  
 $Y = \beta_0 + \beta_1 X_{1} + \beta_2 X_{2} + ... + \beta_k X_{k}$
 
  * <span style="color:red">다중공선성</span>의 문제 발생
      * 회귀 모형에 포함될 X들간의 밀접한 상관관계로 인해 변수 각각의 개별 효과를 파악하기 힘들게 되는 현상 
<br/>

 
 
 | 문제점 	| <span style = "color:blue">대안책</span> 	|
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:	|:-------------------------------------------------:	|
| 1. 예측값의 분산이 커져 회귀 모형의 적합성이 감소한다.<br/> 2. 다른 중요한 독립변수를 제거할 가능성이 증가한다. <br/>3. 과적합의 가능성이 있다.<br/>4. 설명력은 좋지만 과적합으로 인한 예측력의 감소 가능성이 있다.  	| PCA <br/> (Principal Component Analysis : 주성분 분석) 	|


<br/>
<br/>

## 2. PCA(Principal Component Analysis)  
  * 다중공선성이 있는 고차원 공간의 표본들을 다중공선성이 없는 저차원 공간으로 변환하는 것  

  | PCA의 효과 	| 한계점 	| <span style = "color:blue">대안책</span> 	|
|:--------------------------------------------------------------------------------------------------------------------------------------------------:	|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:	|:----------------------------------------------:	|
| 1. 다중 공선성 제거 효과가 있다.  <br/>2. 원 변수와 달리, 자료의 정보를 가장 많이 가지고 있는 축의 방향을 알 수 있다  <br/>3. 차원 축소 효과를 얻을 수 있다.  	| 1. X간의 관계로만 구하기 때문에, 반응변수 Y는 PC의 방향을 정하는데 사용되지 않는다.(<span style="color:red">Y가 배제됨</span>) <br/>  2. 즉, 설명변수를 가장 잘 설명하는 방향이, 반응변수를 예측하기 위해 사용하는 최고의 방향이 될 보장이 없다. 	| PLS <br/> (Partial Least Square  : 부분 최소 제곱법) 	|
<br/>

<br/>

## 3. PLS(Partial Least Square)  


> $X = TP^T + E$  
> $Y = UQ^T + F$  
  (T,U : p개의 추출된 score vector)    
  (P,Q : loading행렬)  
  (E,F : 잔차행렬)  
  
<br/>

####PLS를 계산하는 두가지 알고리즘 : NIPALS, SIMPLS  
<br/>

### 1. NIPALS Algorithm  
가중치 벡터(weight vector) w, c를 사용한다.  

<br/>
$[cov(t, u)]^2 = [cov(X_w, Y_c)]^2 = max_{|r|=|s|=1}[Cov(X_r, Y_s)]^2$  
X, Y의 관계를 알기 위해 t, u(score vector)를 사용하고, 공분산이 최대가 되는 w, c를 찾고자 한다.  
<br/>

> Canonical Ridge Analysis에서 정의한 X와 Y의 공분산행렬  
> <br/>$K = ((1-k_x)X^TX+k_xI)^{-\frac{1}{2}} X^TY((1-k_y)Y^TY+k_yI)^{-\frac{1}{2}}$   
> <br/>$K^2 = ((1-k_x)X^TX+k_xI)^{-1} X^TY((1-k_y)Y^TY+k_yI)^{-1}$  
> <br/>이때, $([1-\gamma_x]var(X_r) + \gamma_x)$는 $Var(X_r)$의 일반화된 식이므로   
> <br/>$((1-k_x)X^TX+k_xI)$ 는 $Var(X_r)$로,  $((1-k_y)Y^TY+k_yI)$ 는 $Var(Y_s)$로 치환한다.  
> <br/>$K^2 = Var(X_r)^{-1} X^TYVar(Y_s)^{-1}Y^TX$  
> <br/>$= (X^TX)^{-1}X^TY(Y^TY)^{-1}Y^TX$  
> <br/>$= X^{-1}(X^T)^{-1}X^TYY^{-1}(Y^T)^{-1}Y^TX$  ($(X^T)^{-1}X^T = I$, $(Y^T)^{-1}Y^T = I$)  
> <br/>$= X^{-1}YY^{-1}X$  (직교행렬 $X^{-1} = X^T$)  
> <br/>$= X^TYY^TX$  
<br/>

**$X^TYY^TXw = \lambda w$**  
w : X를 T-space로 투영시키기 위한 vector라고 가정, c : Y를 U-space로 투영시키기 위한 vector라고 가정    
$t = Xw$ -> $w = X^{-1}t$  
여기서 X와 Y의 관계를 알기 위해 t대신 u를 사용한다.  
<br/>
<br/>

####1-1. NIPALS 알고리즘 과정  
<br/>

> <br/>1) Y의 열벡터를 랜덤하게 선택 (= u)  
> <br/>2) $w = \frac{X^Tu}{u^Tu}$  (weight vector)  
> <br/>3) $w = \frac{w}{||w||}$  (정규화)  
> <br/>4) $t = Xw$  (X의 score vector)  
> <br/>5) $c = \frac{Y^Tt}{t^Tt}$  (weight vector)  
> <br/>6) $c = \frac{c}{||c||}$  (정규화)  
> <br/>7) $u = Yc$  (Y의 score vector)  
> <br/>8) $q = \frac{Y^Tu}{u^Tu}$  (Y의 loading vector)  
> <br/>9) $p = \frac{X^Tt}{t^Tt}$  (X의 loading vector)  
> <br/>10) $p = \frac{p}{||p||}$  (정규화)  
> <br/>11) $X = X - tp^T$, $Y = Y - uq^T$  (행렬을 블록화*)  
> (* : X, Y를 묶어서 하나의 행렬으로 블록화하는 것)  
>이 과정을 $W_{new}$와 $W_{old}$의 차이가 거의 없을때까지 반복  

<br/>

(참고)  
  <br/>* PCA : 주성분의 분산이 최대가 되도록 한다  
    <br/>* $max _{|r|=1} [Var(Xr)]$  
    <br/> ($var(t) = \frac{t^Tt}{n}$)   
  <br/>* CCA : X, Y의 상관관계가 최대가 되도록 한다  
    <br/>* $max _{|r|=|s|=1}[Corr(X_r, Y_s)]^2$  
    <br/> ($[Corr(t, u)]^2 = \tfrac{[Cov(t, u)]^2} {Var(t)Var(u)}$)  
  <br/>* PLS : X, Y의 공분산이 최대가 되도록 한다.
    <br/>* $max_{|r|=|s|=1}[Cov(X_r, Y_s)]^2 = max_{|r|=|s|_1}Var(X_r)[Corr(X_r, Y_r)]^2 Var(Y_s)$  
    <br/> ($cov(t, u) = \frac{t^T u}{n}$)  
    

<br/>
<br/>

####1-2. PLS Regression  

- PLS1(Single Y를 block) 과 PLS2(Multiple Y를 block)가 선형회귀 문제를 푸는데 사용된다.
- 가정1. score vector t로 Y를 잘 예측할 수 있다.  
- 가정2. score vector t와 u는 선형 관계에 있다.  
  $U = TD + H$   
  (D: (p*p) 직교 행렬)  
  (H: 잔차행렬)  
  
- 회귀 계수를 구할 때 참조 할 식  
  1) $X = TP^T + E$
  2) $Y = UQ^T + F$
  3) $w = \frac{X^Tu}{u^Tu}$
  4) $c = \frac{Y^Tt}{t^Tt}$
  5) $t = Xw$
  6) $u = Yc$
  
>**회귀계수 유도 과정**  
<br/>
> <br/>- 2번식 $Y = UQ^T + F$에 $U = TD +H$를 대입  
>   <br/>$= (TD + H)Q^T + F$  
>   <br/>$= T(DQ^T) + (HQ^T + F)$  
> <br/>- 4번식이 $C = Y^TT$ -> $Y = TC^T$이므로 재정의  
>   <br/>$Y = TC^T + F^*$...(7) ($C^T = DQ^T$, 잔차 $F^* = HQ^T + F$)  
> <br/>- 1번식 $X = TP^T$ -> $T = XP$를 (7)에 대입  
>   <br/>$Y = (XP)C^T + F^*$...(8)  
> <br/>- X와 Y의 선형식을 $Y = XB + F^*$(NIPALS 회귀모형)라고 하면 $B = X^{-1}Y$이다. 이 식에 (8)을 대입  
>  <br/>$B = X^{-1}(XP)C^T$  
>   <br/>   = $PC^T$  ($P=X^TT$이고 $C^T = T^TY$이므로)  
>  <br/>   = $X^TTT^TY$  
  
PLS회귀의 추정값 : $\hat{Y} = XB = X(X^TTT^TY) = T(T^TY) = TC^T$  
(X : data, T : score vector)  
<br/>
<br/>

### 2. SIMPLS Algorithm  

- Y가 한 개일 때는 PLS1 회귀분석과 같지만 다변수 Y에 대해서는 SIMPLS와 NIPALS-PLS2사이에 약간의 차이 존재
- NIPALS와 같은 <span style="color:red">수축된 data matrix 구축 불필요</span> -> Data수축과정을 포함하지 않기에 빠르고 쉬운 해석 가능  
- 원 변수의 선형조합으로 PLS계수 직접 계산 가능  
<br/>

####2-1. SIMPLS 알고리즘 과정  
<br/>

> i=1, $S = X^TY$를 SVD함  
> i>1, $S = S-P(P^TP)^{-1}P^TS$를 SVD함  
> $w = u(,1)$ (u의 첫번째 column) (u : SVD(S))  
> $t = Xw$  
> $p = \frac{X^Tt}{t^Tt}$  
> $B = W(T^TT)^{-1}T^TY$ (회귀계수)  

