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

 Using a little bit of algebra, prove that (4.2) is equivalent to (4.3). In other words, the logistic function representation and logit representation for the logistic regression model are quivalent.


`정답`
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

It was stated in the text that classifying an observation to the class for which (4.17) is largest is equivalent to classifying an observation to the class for which (4.18) is largest. Prove that this is the case. In other words, under the assumption that the observations in the kth class are drawn from a N(µk, σ2) distribution, the Bayes classifer assigns an obser



<br>

## 5번
We now examine the diferences between LDA and QDA.
(a) If the Bayes decision boundary is linear, do we expect LDA or
QDA to perform better on the training set? On the test set?
(b) If the Bayes decision boundary is non-linear, do we expect LDA
or QDA to perform better on the training set? On the test set?
(c) In general, as the sample size n increases, do we expect the test
prediction accuracy of QDA relative to LDA to improve, decline,
or be unchanged? Why?
(d) True or False: Even if the Bayes decision boundary for a given
problem is linear, we will probably achieve a superior test error rate using QDA rather than LDA because QDA is fex enough to model a linear decision boundary. Justify your answer.


## 6번

. Suppose we collect data for a group of students in a statistics class
with variables X1 = hours studied, X2 = undergrad GPA, and Y =
receive an A. We ft a logistic regression and produce estimated
coefcient, βˆ0 = −6, βˆ1 = 0.05, βˆ2 = 1.
(a) Estimate the probability that a student who studies for 40 h and
has an undergrad GPA of 3.5 gets an A in the class.
(b) How many hours would the student in part (a) need to study to
have a 50 % chance of getting an A in the class

## 13번 (b~i)

This question should be answered using the Weekly data set, which
is part of the ISLR2 package. This data is similar in nature to the
Smarket data from this chapter’s lab, except that it contains 1, 089
weekly returns for 21 years, from the beginning of 1990 to the end of
2010


(b) Use the full data set to perform a logistic regression with
Direction as the response and the fve lag variables plus Volume
as predictors. Use the summary function to print the results. Do
any of the predictors appear to be statistically signifcant? If so,
which ones?
(c) Compute the confusion matrix and overall fraction of correct
predictions. Explain what the confusion matrix is telling you
about the types of mistakes made by logistic regression.
(d) Now ft the logistic regression model using a training data period
from 1990 to 2008, with Lag2 as the only predictor. Compute the
confusion matrix and the overall fraction of correct predictions
for the held out data (that is, the data from 2009 and 2010).
(e) Repeat (d) using LDA.
(f) Repeat (d) using QDA.
(g) Repeat (d) using KNN with K = 1.
(h) Repeat (d) using naive Bayes.
(i) Which of these methods appears to provide the best results on
this data?


## 14번

In this problem, you will develop a model to predict whether a given
car gets high or low gas mileage based on the Auto data set.
(a) Create a binary variable, mpg01, that contains a 1 if mpg contains
a value above its median, and a 0 if mpg contains a value below
its median. You can compute the median using the median()
function. Note you may fnd it helpful to use the data.frame()
function to create a single data set containing both mpg01 and
the other Auto variables.
(b) Explore the data graphically in order to investigate the association between mpg01 and the other features. Which of the other
features seem most likely to be useful in predicting mpg01? Scatterplots and boxplots may be useful tools to answer this question. Describe your fndings.
(c) Split the data into a training set and a test set.
(d) Perform LDA on the training data in order to predict mpg01
using the variables that seemed most associated with mpg01 in
(b). What is the test error of the model obtained (e) Perform QDA on the training data in order to predict mpg01
using the variables that seemed most associated with mpg01 in
(b). What is the test error of the model obtained?
(f) Perform logistic regression on the training data in order to predict mpg01 using the variables that seemed most associated with
mpg01 in (b). What is the test error of the model obtained?
(g) Perform naive Bayes on the training data in order to predict
mpg01 using the variables that seemed most associated with mpg01
in (b). What is the test error of the model obtained?
(h) Perform KNN on the training data, with several values of K, in
order to predict mpg01. Use only the variables that seemed most
associated with mpg01 in (b). What test errors do you obtain?
Which value of K seems to perform the best on this data set

## 16번

. Using the Boston data set, ft classifcation models in order to predict
whether a given census tract has a crime rate above or below the median. Explore logistic regression, LDA, naive Bayes, and KNN models
using various subsets of the predictors. Describe your fndings.
Hint: You will have to create the response variable yourself, using the
variables that are contained in the Boston data set



<br><br><br>
끝🙂
<br><br><br>