---
layout: article
title: Classification with R
permalink: /notes/kr/algorithm/Ch06_classfications_R
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
mermaid: true
mathjax: true

---


*해당 포스팅은 'an introduction to statistical learning' 책의 연습문제를 R로 해석하며 공부한 것을 정리해두었습니다*


# Classification_4.8 Exercises


## 1번


`Question`

 Using a little bit of algebra, prove that (4.2) is equivalent to (4.3). In other words, the logistic function representation and logit representation for the logistic regression model are quivalent.


`Answer`

1. 로지스틱 함수 표현?
 로지스틱 함수는 앞서 보았듯, 주어진 입력 X에 대한 이벤트가 발생할 확률을 나타낸다. 즉, 그 결과는 0과 1 사이의 값이다. 그래서 시그모이드 함수로 표현하는 것을 보았다.

`(4.2)`

 $$ p(X) = \frac{e^{\beta_0 + \beta_1 X}}{1 + e^{\beta_0 + \beta_1 X}} $$


2. 로짓 표현?
 *로짓 표현은 확률을 로그 오즈로 변환한 것*으로, 모든 실수 값을 가질 수 있다. 로지스틱 회귀 분석에서는 로짓 표현을 사용하여 종속 변수와 독립 변수 간의 선형 관계를 설명할 수 있다.

위의 함수를 아래와 같이도 표현할 수 있다.


$$ \frac{p(X)}{1-p(X)} = e^{\beta_0 + \beta_1 X} $$



여기서, 

$$ \frac{p(X)}{1-p(X)} $$

는 odds(공산)이라하며 항상 0과 무한대 사이의 값을 가진다. 


여기서 양변에 로그를 취하면 아래와 같다.



`(4.3)`

$$ \log\left(\frac{p(X)}{1-p(X)}\right) = \beta_0 + \beta_1 X $$



**따라서** 
로지스틱 함수를 사용하여 로짓 표현을 만들 수 있고, 로짓 표현을 가지고 로지스틱 함수를 쓸 수 있기 때문에 4.2와 4.3은 같다고 할 수 있다. 로지스틱에서는 결과 해석이나 예측을 이것들을 활용해서 잘 할 수 있다고 한다...




<br>

## 2번

<br>

`Question`

It was stated in the text that classifying an observation to the class for which (4.17) is largest is equivalent to classifying an observation to the class for which (4.18) is largest. Prove that this is the case. In other words, under the assumption that the observations in the kth class are drawn from a N(µk, σ2) distribution, the Bayes classifer assigns an obser

`(4.17)`
$$
p_k(x) = \frac{\pi_k \frac{1}{\sqrt{2\pi\sigma}} \exp\left(-\frac{1}{2\sigma^2}(x-\mu_k)^2\right)}{\sum_{l=1}^{K} \pi_l \frac{1}{\sqrt{2\pi\sigma}} \exp\left(-\frac{1}{2\sigma^2}(x-\mu_l)^2\right)}
$$


`(4.18)` 
$$
\delta_k(x) = x \cdot \frac{\mu_k}{\sigma^2} - \frac{\mu_k^2}{2\sigma^2} + \log(\pi_k)
$$


`Answer`

분류에서는 각 관측치(데이터 포인트)를 특정 클래스(그룹)에 할당하는 것이 목표이다. 그러므로, 우리는 각 관측치가 각 클래스에 속할 확률을 계산해야 한다. 확률을 최대화하는 클래스에 관측치를 할당 한다고 말할 수 있다. 


이 문제 목표는 `(4.17)`식을 최대화하는 클래스에 관측치를 할당하는 것이 `(4.18)`를 최대화하는 클래스에 관측치를 할당하는 것과 동일하다는 것을 증명해야 한다.

그럼, 먼저 4.17에 로그를 취해도, 동일하게 최대화가 된다.

$$
\log(p_k(x)) = \log(\pi_k) + \log\left(\exp\left(-\frac{1}{2\sigma^2}(x-\mu_k)^2\right)\right) - \log\left(\sum_{l=1}^{K} \pi_l \exp\left(-\frac{1}{2\sigma^2}(x-\mu_l)^2\right)\right)
$$

이제, 지수함수의 로그는 아래와 같다고 볼 수 있으므로
$$
\log(\exp(a)) = a
$$

적용하면:
$$
\log(p_k(x)) = \log(\pi_k) - \frac{1}{2\sigma^2}(x-\mu_k)^2 + C
$$
(C는 상수)

이 식의 선형 부분만 고려하면 4.18과 비슷한 식이 된다. 
$$
\log(p_k(x)) \propto x\mu_k - \frac{\mu_k^2}{2\sigma^2} + \log(\pi_k)
$$


따라서, 4.17 식을 최대화하는 클래스에 관측치를 할당하는 것은, 4.18에 할당하는것과 동일하다고 할 수 있따. 


```
로그 변환: 로그 함수는 곱셈을 덧셈으로 바꾸는 특성있으며, 복잡한 확률 계산을 단순화하기 위해 로그를 사용하는 경우가 많음.

베이즈 분류기: 데이터 포인트가 어떤 클래스에 속하는지 결정하기 위해, 각 클래스에 대한 데이터 포인트의 확률을 계산하고 가장 높은 확률을 가진 클래스를 선택


- 수식 의미
​
p_k(x): 이것은 데이터 포인트 x가 k번째 클래스에 속할 확률을 나타냄


π_k: k째 클래스의 사전 확률로 아무런 정보 없이 k번째 클래스에 속할 기본 확률을 의미


μ_k와 σ: k번째 클래스의 평균과 표준편차로 각 클래스의 데이터 분포를 나타냄. 데이터 포인트 x가 어떤 클래스에 속할지 결정하기 위해 사용되며, 로그 변환은 이러한 확률 계산을 단순화하기 위해 사용됨.


간단히 말해, 이 수식들은 주어진 데이터 포인트가 어떤 그룹에 속하는지 가장 확률이 높은지를 결정하기위해 사용됨.
```


<br>




## 5번 - LDA and QDA

`Question`

We now examine the diferences between LDA and QDA.


(a) If the Bayes decision boundary is linear, do we expect LDA or QDA to perform better on the training set? On the test set?


`Answer`


LDA가 선형 decision boundary를 모델링하므로, Bayes decision boundary가 선형일 때는 QDA보다 LDA가 훈련 세트와 테스트 세트 모두 더 좋은 성능을 보일 것으로 예상된다.

다만, 훈련세트에서 QDA가 오버피팅할 가능성이 있기 때문에 QDA가 나은 성능을 보일수도 있지만, 결국 선형 decision boundary를 모델링하는데 적합한 LDA가 더 나은 성능을 보일 것으로 예상된다.


```
LDA (Linear Discriminant Analysis): 각 클래스의 데이터가 동일한 공분산 구조를 가진다고 가정하고, 선형 결정 경계를 생성

QDA (Quadratic Discriminant Analysis): 각 클래스의 데이터가 다른 공분산 구조를 가질 수 있다고 가정하고, 비선형 결정 경계를 생성
```


`Question`

(b) If the Bayes decision boundary is non-linear, do we expect LDA or QDA to perform better on the training set? On the test set?



`Answer`
위 A번과 달리, Bayes의 바운더리가 비선형인 경우 QDA가 비선형 decision boundary를 모델링할 수 있으므로 QDA가의 성능이 더 좋을 것이다.


`Question`

(c) In general, as the sample size n increases, do we expect the test prediction accuracy of QDA relative to LDA to improve, decline, or be unchanged? Why?


`Answer`

샘플 사이즈 n이 증가하면, QDA의 성능이 더 향상될 것이다. QDA는 모델이 복잡하기 때문에 더 많은 데이터가 필요하고, 샘플 데이터의 사이즈가 더 커진다면 복잡한 패턴을 학습해서 더 좋은 성능을 낼 것이기 때문이다.



`Question`

(d) True or False: Even if the Bayes decision boundary for a given problem is linear, we will probably achieve a superior test error rate using QDA rather than LDA because QDA is fex enough to model a linear decision boundary. Justify your answer.



`Answer`

Bayes decision boundary가 선형인 경우 LDA가 더 간단하고 적절한 모델이므로 더 나은 성능을 가져올 것이다. QDA는 모델이 복잡하므로 과적합의 위험이 있을 수 있다.



<br>

## 6번

`Question`

Suppose we collect data for a group of students in a statistics class with variables X1 = hours studied, X2 = undergrad GPA, and Y = receive an A. We ft a logistic regression and produce estimated coefcient, βˆ0 = −6, βˆ1 = 0.05, βˆ2 = 1.


(a) Estimate the probability that a student who studies for 40 h and has an undergrad GPA of 3.5 gets an A in the class.



`Answer`

```r
beta0 <- -6
beta1 <- 0.05
beta2 <- 1

log_odds_a <- beta0 + beta1*40 + beta2*3.5
probability_a <- exp(log_odds_a) / (1 + exp(log_odds_a))
probability_a
```

> 결과

[1] 0.3775407

A grade를 받을 확률은 37.75%



`Question`

(b) How many hours would the student in part (a) need to study to have a 50 % chance of getting an A in the class



`Answer`

```r
need_hours <- function(hours, beta0, beta1, beta2) {
  log_odds_b <- beta0 + beta1*hours + beta2*3.5
  probability_b <- exp(log_odds_b) / (1 + exp(log_odds_b))
  return(probability_b - 0.5)
}

result <- uniroot(need_hours, c(0, 100), beta0=beta0, beta1=beta1, beta2=beta2)
required_hours <- result$root
required_hours
```

> 결과
50


A를 받으려면, 50시간의 공부가 필요하다.

```
- need_hours 함수:
hours 값에 대해 확률을 계산하고 0.5(50%)를 빼는 값을 반환
로지스틱 함수의 출력 확률을 계산하고, 해당 확률에서 50%를 빼면 0에 가까운 값이 나올 것. 즉, 함수의 출력 값이 0에 가까워지면 그 hours 값은 50% 확률에 가까워진다는 것을 의미

- uniroot 함수:
uniroot는 주어진 함수의 근(루트)을 찾는 R의 내장 함수. need_hours 함수의 출력 값이 0에 가까운 hours 값을 찾고자 함. c(0, 100)은 hours 값의 가능한 범위를 설정했고, 0시간부터 100시간 사이로 설정함. beta0=beta0, beta1=beta1, beta2=beta2는 find_hours 함수에 필요한 추가 인수를 제공

- result$root:
uniroot 함수의 결과는 여러 속성을 가진 목록 형태로 반환. root 속성은 찾은 근(루트)을 나타냄. 50%의 확률에 해당하는 공부 시간
```


<br>

## 13번 (b~i)

`Question`

This question should be answered using the Weekly data set, which is part of the ISLR2 package. This data is similar in nature to the Smarket data from this chapter’s lab, except that it contains 1, 089 weekly returns for 21 years, from the beginning of 1990 to the end of 2010


(b) Use the full data set to perform a logistic regression with Direction as the response and the fve lag variables plus Volume as predictors. Use the summary function to print the results. Do any of the predictors appear to be statistically signifcant? If so, which ones? 



`Answer`

```r
library(ISLR2)
data(Weekly)

fit_logistic <- glm(Direction ~ Lag1 + Lag2 + Lag3 + Lag4 + Lag5 + Volume, data=Weekly, family=binomial)
summary(fit_logistic)
```

![Alt text](img/logistic1.png)

intercept와 lag2만 유의미한 변수인 것 같다.



`Question`

(c) Compute the confusion matrix and overall fraction of correct predictions. Explain what the confusion matrix is telling you about the types of mistakes made by logistic regression.



`Answer`

```r
pred_logistic <- predict(fit_logistic, type="response")
predicted_direction <- ifelse(pred_logistic > 0.5, "Up", "Down")
table(Weekly$Direction, predicted_direction)
```

>결과
```
       predicted_direction
       Down  Up
  Down   54 430
  Up     48 557
  ```

TP: 실제로 'Up'이었고 모델도 'Up'으로 예측한 경우- 557번
TN: 실제로 'Down'이었고 모델도 'Down'으로 예측한 경우 54번
FP: 실제로 'Down'이었지만 모델이 'Up'으로 잘못 예측한 경우 430번
FN: 실제로 'Up'이었지만 모델이 'Down'으로 잘못 예측한 경우는48번

Accuracy = $\frac{TP + TN}{TP + TN + FP + FN}$

 56.1%의 정확도가 도출됨.



> r로 정확도 계산하기

```
matrix_data <- matrix(c(54, 48, 430, 557), ncol=2)

rownames(matrix_data) <- c("Down", "Up")

colnames(matrix_data) <- c("Down", "Up")

accuracy <- sum(diag(matrix_data)) / sum(matrix_data)

accuracy
```



`Question`

(d) Now ft the logistic regression model using a training data period from 1990 to 2008, with Lag2 as the only predictor. Compute the confusion matrix and the overall fraction of correct predictions for the held out data (that is, the data from 2009 and 2010).


`Answer`

1990~2008년까지 훈련데이터로, 2009~2010년까지 테스트데이터로 분할하고, Lag2에 대해 특정 예측변수만 사용해서 로지스틱 회귀 모델로 학습시켜야한다. 이후 성능 평가를 위해 confusion matrix를 생성하여 모델 예측도와 오류 유형을 파악해야한다. 이후 전체 정확도 계산.


훈련 / 테스트 셋 분할

```r
train <- subset(Weekly, Year < 2009)
test <- subset(Weekly, Year >= 2009)
```


로지스틱 회귀 돌리기

```r
fit_logistic_d <- glm(Direction ~ Lag2, data=train, family=binomial)
pred_logistic_d <- predict(fit_logistic_d, newdata=test, type="response")
predicted_direction_d <- ifelse(pred_logistic_d > 0.5, "Up", "Down")
```


Confusion matrix 및 정확도 계산

```r
conf_matrix <- table(test$Direction, predicted_direction_d)
accuracy <- sum(diag(conf_matrix)) / sum(conf_matrix)
conf_matrix
accuracy
```

>결과

[1] 0.625

로지스틱 회귀 모델 62.5%의 정확도가 나옴.




`Question`

(e) Repeat (d) using LDA.

`Answer`

LDA모델로 train data 훈련

```r
set.seed(123) 
lda<- train(Direction ~ Lag2, data=train, method="lda")
```

학습된 LDA 모델을 사용하여 테스트 데이터의 예측
```r
lda_pred <- predict(lda_fit, test)$class
```

Confusion matrix와 전체 정확도를 계산
```r
conf_matrix_lda <- confusionMatrix(lda_pred, test$Direction)
conf_matrix_lda$table
conf_matrix_lda$overall['Accuracy']
```

>결과
  0.625 

62.5%의 정확도로 나타남




`Question`

(f) Repeat (d) using QDA.

`Answer`


QDA 모델을 훈련 데이터로 학습

```r
set.seed(123)
qda_fit <- train(Direction ~ Lag2, data=train, method="qda")
```

QDA 모델을 사용하여 테스트 데이터의 예측
```r
qda_pred <- predict(qda_fit, newdata=test)
```

Confusion matrix와 전체 정확도를 계산

```r
conf_matrix_qda <- confusionMatrix(qda_pred, test$Direction)
conf_matrix_qda$table
conf_matrix_qda$overall['Accuracy']
```

>결과
0.5865385 

58.6%의 정확도로 예측함. LDA보다 낮은 예측률 보임.




`Question`

(g) Repeat (d) using KNN with K = 1.

`Answer`

```r
train_X <- as.matrix(train$Lag2)
test_X <- as.matrix(test$Lag2)
train_Y <- train$Direction
```

```r
set.seed(123) # 재현 가능성을 위한 시드 설정
knn_pred <- knn(train_X, test_X, train_Y, k=1)
```

```r
conf_matrix_knn <- table(test$Direction, knn_pred)
accuracy_knn <- sum(diag(conf_matrix_knn)) / sum(conf_matrix_knn)
conf_matrix_knn
accuracy_knn
```

>결과
      knn_pred

       Down Up

  Down   21 22

  Up     29 32

> accuracy_knn
[1] 0.5096154

50.9%의 정확도로 예측함. 지금까지의 모델 중에선 KNN 모델이 가장 낮은 성능을 보이는 것으로 보임.




`Question`

(h) Repeat (d) using naive Bayes.


`Answer`

베이즈 쓰기 위해 install.packages("e1071"). 나이브 베이즈는 텍스트 분류에 자주 쓰임. 베이즈 정리 기반으로한 확률적 분류며 알다시피 조건부 독립을 가정해서 naive라고 함. 특징간 관계가 없다고 간주하고 계산이 단순화되어 효율적으로 처리됨.


```r
library(e1071)

nb_fit <- naiveBayes(Direction ~ Lag2, data=train)

nb_pred <- predict(nb_fit, newdata=test)

conf_matrix_nb <- table(test$Direction, nb_pred)
accuracy_nb <- sum(diag(conf_matrix_nb)) / sum(conf_matrix_nb)
conf_matrix_nb
accuracy_nb
```

>결과
[1] 0.5865385

58%의 정확도로 예측함.




`Question`

(i) Which of these methods appears to provide the best results on this data?


`Answer`

로지스틱과 LDA가 62.%의 정확도를 보이고, 다음으로 QDA, NB가 58.65%, KNN은 50%의 정확도를 보였다.



<br>

## 14번

`Question`

In this problem, you will develop a model to predict whether a given car gets high or low gas mileage based on the Auto data set.

(a) Create a binary variable, mpg01, that contains a 1 if mpg contains a value above its median, and a 0 if mpg contains a value below its median. You can compute the median using the median() function. Note you may fnd it helpful to use the data.frame() function to create a single data set containing both mpg01 and the other Auto variables.



`Answer`

 Auto 데이터셋의 mpg변수를 기반으로 고/저 연료 효율의 자동차를 예측하기 위한 이진 변수 mpg01을 생성해야 함. mpg의 값이 중앙값보다 높으면 mpg01은 1을, 그렇지 않으면 0을 가져야하고, 중앙값은 median() 함수를 사용하여 계산해라..

Auto는 ISLR에 있었따.. 

```r
library(ISLR)
head(Auto)
```


변수 중앙값과 mpg01의 이진변수 생성

```r
mpg_median <- median(Auto$mpg)
Auto$mpg01 <- ifelse(Auto$mpg > mpg_median, 1, 0)
```



`Question`

(b) Explore the data graphically in order to investigate the association between mpg01 and the other features. Which of the other features seem most likely to be useful in predicting mpg01? Scatterplots and boxplots may be useful tools to answer this question. Describe your fndings.


연속형 변수니, pari()를 써서 산점도를 그렸다.

```r
pairs(Auto[, -which(names(Auto) %in% c("mpg", "mpg01"))], col=Auto$mpg01+1)
```


![Alt text](img/logistic3.png)


**색깔**

붉은색: mpg가 중앙값보다 높은 경우

검은색: mpg가 중앙값보다 낮은 경우

**가로축과 세로축**

각 작은 그래프의 가로축과 세로축은 변수를 나타낸다.

예를 들어, 'displacement'와 'horsepower' 사이의 그래프는 'displacement'의 값이 증가함에 따라 'horsepower'가 어떻게 변하는지 보여준.

**변수 간의 관계**

그래프에서 점들이 어떻게 분포되어 있는지를 보면 두 변수 사이의 관계를 알 수 있다. 점들이 오른쪽 위로 기울어져 있으면 양의 상관관계, 왼쪽 위로 기울어져 있으면 음의 상관관계를 가진다고 할 수 있다


**!어떤 변수가 mpg01 예측에 도움이 될까?**

붉은색과 검은색 점들이 명확하게 분리되어 있는 변수는 mpg01을 예측하는 데 도움이 될 가능성이 있다.
예를 들어, weight 변수에서는 무거운 차량이 주로 mpg의 중앙값 아래에 있고, 가벼운 차량이 중앙값 위에 있다.




`Question`

(c) Split the data into a training set and a test set.


```r
set.seed(123)
index <- sample(1:nrow(Auto), nrow(Auto)*0.7)

train <- Auto[index, ]
test <- Auto[-index, ]
```

`Question`

(d) Perform LDA on the training data in order to predict mpg01 using the variables that seemed most associated with mpg01 in (b). What is the test error of the model obtained 


`Answer`

LDA모델 학습

```r
lda_model <- lda(mpg01 ~ weight + horsepower + displacement, data=train)
```


예측

```r
lda_pred <- predict(lda_model, newdata=test)$class
```

에러 계산

```r
test_error <- mean(lda_pred != test$mpg01)
test_error
```

>결과

[1] 0.1186441

에러가 11.8%이므로, 정확도는 88.14%로 mpg01을 예측한 결과가 나옴.

weight, horsepower, displacement가 유용한 변수로 나옴.


`Question`

(e) Perform QDA on the training data in order to predict mpg01 using the variables that seemed most associated with mpg01 in (b). What is the test error of the model obtained?


`Answer`

이번엔 각 클래스에 대해 공분산 행렬 추정하고, QDA이므로 비선형 경계가 나타날 수 있음.

```r
qda_fit <- qda(mpg01 ~ weight + horsepower + displacement, data = train)
qda_pred <- predict(qda_fit, test)$class
test_error <- mean(qda_pred != test$mpg01)
test_error
```

>결과

[1] 0.1016949

테스트 에러는 10.17%로 LDA보다 조금 더 나은 성능을 보인다. LDA는 모든 클래스에 대해 동일한 공분산, QDA는 각 클래스마다 공분산을 가진 결과인 것 같다.



`Question`

(f) Perform logistic regression on the training data in order to predict mpg01 using the variables that seemed most associated with mpg01 in (b). What is the test error of the model obtained?


`Answer`

```r
logistic_fit <- glm(mpg01 ~ weight + year + displacement + horsepower, data=train, family=binomial)
logistic_pred <- predict(logistic_fit, test, type="response")
logistic_pred_class <- ifelse(logistic_pred > 0.5, 1, 0)
logistic_error <- mean(logistic_pred_class != test$mpg01)
logistic_error
```

>결과

[1] 0.1186441



`Question`

(g) Perform naive Bayes on the training data in order to predict mpg01 using the variables that seemed most associated with mpg01 in (b). What is the test error of the model obtained?


`Answer`

```r
naive_bayes_fit <- naiveBayes(mpg01 ~ weight + year + displacement + horsepower, data=train)
naive_bayes_pred <- predict(naive_bayes_fit, test)
naive_bayes_error <- mean(naive_bayes_pred != test$mpg01)
naive_bayes_error
```

>결과

[1] 0.1186441


로지스틱하고 나이브베이즈의 동일 에러결과가 나왔다.

로지스틱 회귀는 입력 변수와 출력 간의 선형 관계를 가정하며, 이 관계를 사용하여 확률을 예측

나이브 베이즈는 각 특성이 독립적이라는 (나이브한) 가정을 기반으로 하며, 베이즈의 정리를 사용하여 확률을 업데이트

두 모델이 동일한 테스트 에러를 보이는 것은, 이 데이터에 대해 두 방법 모두 비슷한 예측 성능을 가지고 있음. 하지만 데이터의 특성과 분포, 그리고 문제의 복잡도 등 여러 요인에 따라 달라질 수 있음.



`Question`

(h) Perform KNN on the training data, with several values of K, in order to predict mpg01. Use only the variables that seemed most associated with mpg01 in (b). What test errors do you obtain? Which value of K seems to perform the best on this data set

`Answer`

k는 이웃 수. k값이 작으면 모델은 노이즈에 민감하고 크면 decision boundary가 스무스해서 과소적합할 가능성이 있음. 

```r
library(class)

train_X <- train[, c("weight", "year", "displacement", "horsepower")]
test_X <- test[, c("weight", "year", "displacement", "horsepower")]

k_values <- c(1, 3, 5, 7, 9, 11, 13, 15) 
errors <- numeric(length(k_values))

for (i in 1:length(k_values)) {
  set.seed(123)  
  knn_pred <- knn(train_X, test_X, train$mpg01, k=k_values[i])
  errors[i] <- mean(knn_pred != test$mpg01)
}

data.frame(K=k_values, Error=errors)
```

>결과 

  K      Error

1  1 0.16949153

2  3 0.13559322

3  5 0.11016949

4  7 0.10169492

5  9 0.11016949

6 11 0.08474576

7 13 0.09322034

8 15 0.10169492

11일 때가 가장 낮은 8.4%의 에러를 보임. k=11



<br>

## 16번

`Question`

Using the Boston data set, ft classifcation models in order to predict whether a given census tract has a crime rate above or below the median. Explore logistic regression, LDA, naive Bayes, and KNN models using various subsets of the predictors. Describe your fndings. 

>Hint: You will have to create the response variable yourself, using the variables that are contained in the Boston data set



`Answer`

 Boston 데이터셋을 사용하여 인구 조사 구역의 범죄율이 중앙값보다 높은지 또는 낮은지를 예측하기 위한 분류 모델을 구축하여 위와 동일하게 로지스틱, LDA, NB, KNN 모델 탐색해야한당..


일단 불러오고 변수 생성

```r
library(MASS)
crim_median <- median(Boston$crim)
Boston$crim01 <- ifelse(Boston$crim > crim_median, 1, 0)
```

로지스틱
```r
glm_fit <- glm(crim01 ~ . - crim, data=Boston, family=binomial)
summary(glm_fit)
```

![Alt text](img/logistic2.png)

zn, nox, dis, rad, ptratio, tax, black, medv 변수가 유의한 것 같다. 

모델 적합도를 평가하는 Residual deviance가 Null deviance보다 훨씬 작으면 모델이 데이터에 잘 적합되었다고 볼 수 있음.   Null deviance는 701.46, Residual deviance는 211.93으로 모델이 데이터에 상당히 잘 적합되었다고 보면 된다. 즉, Boston 데이터셋의 여러 변수들을 기반으로 범죄율이 중앙값보다 높을 확률을 예측한다.






<br><br><br>
끝🙂
<br><br><br>


