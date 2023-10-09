---
layout: article
title: 12. group 활용하기
permalink: /notes/kr/R/Ch12_ggplot_12
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
mermaid: true
mathjax: true
---





<br>

# group
<br>

## Data Intrdoction : 'Oxboys' data in R

Oxford에 있는 26명 소년에 대한 자료. 해당 소년들이 나이들면, 키가 크는지 보기 위하여 9번에 걸쳐 측정한 자료다. 


library(nlme)에 내장되어 있다.


> 변수

```
- Subject: 소년 ID
– age : 표준화된 나이 (-1 부터 1 의 값)
– height: 키(cm)
– Occasion: 키가 측정된 순서로, 1 은 가장 먼저 측정된 것, 9 는 마지막 측정을 나타는 범주형 변수
```


```r
library(nlme)
library(tidyverse)
data(Oxboys, package = "nlme")
head(Oxboys)
```

![oxboys](img/oxboys1.png)


<br>

## Multiple groups, one aesthetic


ggplot에서 그룹 지정한 것과 그렇지 않은 것을 비교해보고자 한다. 이 자료에서 궁금한 것은 나이별로 키가 어떻게 변화하는지 궁금하다.

```r
ggplot(Oxboys, aes(age, height)) +
  geom_point() + geom_line()
```


![Alt text](img/oxboys2.png)


age별로 sorting해서 이어주고 있다. 여기서 소년 별로 알고 싶을 때, group 옵션을 사용하는 것이다.


이 때, group=subject로 각 아이들의 정보는 자신의 정보랑 묶일 수 있도록 지정해줄 수 있다. 


```r
ggplot(Oxboys, aes(age, height, group=Subject)) +
  geom_point() + geom_line()
```

![Alt text](img/oxboys3.png)


그럼, 각 id(아이들 개별)별로 라인이 연결된다. 그럼, 각 아이들 별로 세부적인 흐름을 살펴볼 수 있다. 


>point는 그룹별로 묶어지지 않지만 line은 묶인다.



여기서, group을 지정하지 않아도, subject를 **color**로 묶어버리면 또 그룹처럼 구분이 가능하기도 하다


```r
ggplot(Oxboys, aes(age, height, color=Subject)) +
  geom_point() + geom_line()
```

![Alt text](img/oxboys4.png)



<br>

## Different groups on different layers
<br>


ggplot에서 지정한 aesthetic mapping은 그 이후에 추가하는 모든 레이어(layer)에 영향을 미친다. 즉, 데이터의 어떤 열을 색상으로 매핑하거나 크기로 매핑한 경우, 이 매핑은 그래프에 추가하는 모든 레이어에 적용된다.

그러나, 레이어 내에서도 aesthetic mapping을 지정할 수 있다. 레이어 내에서 지정된 aesthetic mapping은 해당 레이어 내에서 우선적으로 적용된다. 즉, 레이어 내에서 특정 열을 다른 색상으로 매핑하면, 이 매핑은 해당 레이어에만 적용되며, ggplot에서 지정한 전체적인 aesthetic mapping을 덮어쓸 수 있다.



이에 따라, **group = Subject 를 ggplot에서 지정한 경우와 geom_line에서 지정한 경우의 차이** 비교해보겠다.



```r
ggplot(Oxboys, aes(age, height, group = Subject)) +
  geom_line() +
  geom_smooth(method = "lm", se = FALSE)
```


![Alt text](img/oxboys4.png)



regression을 활용해서 스무딩라인을 그린다. 파란색 선은 선형 회귀선(linear regression line)인데, 데이터 포인트들의 추세를 나타내며, 데이터에 가장 잘 적합하는 선형 모델을 표시하는 것이다.


그리고, 그룹을 ggplot에서 지정했기 때문에, 이후의 geom에 다 걸리게 됨. line도 sub, smoothing도 group별로 다 하게 된다. 


! 근데, 여기서 문제는, 모든 아이들의 최적선을 그리니 복잡하다는 것이다.



그래서, 전체 아이들 키의 최적선을 보고 싶으면 **group을 ggplot이 아닌 line에서 잡아주는 것이다.** 


```r
ggplot(Oxboys, aes(age, height)) +
  geom_line(aes(group = Subject)) +
  geom_smooth(method = "lm", linewidth = 2, se = FALSE)
```

![Alt text](img/oxboys5.png)


전체 아이들의 키 중에서 전체 패턴을 파악해서 최적선을 그리게 된다.



> se=false
```
lm 그리면 옆에 신뢰구간 뜨는데, 그거 뜨지 않게 하는 것임.
se 매개변수를 FALSE로 설정하는 것은 부트스트래핑(bootstrap)된 신뢰 구간(confidence interval)을 표시하지 않도록 하는 것을 의미함.
```



<br>

## Overriding the default grouping
<br>

상자그림 위에 Subject group별로 profile line 그리려고 한다. geom_boxplot()의 경우 x변수는 범주형 변수로 x변수가
group 으로 쓰이게 되며 x변수의 범주별로 상자그림을 그린다. 


```r
```

```r
```


```r
```


```r
```



<br>

## Matching aesthetics to graphic objects
<br>
```r
```


```r
```


```r
```

```r
```



<br>

## Surface plots
<br>

```r
```


```r
```


```r
```


```r
```



<br><br><br>
끝🙂
<br><br><br>