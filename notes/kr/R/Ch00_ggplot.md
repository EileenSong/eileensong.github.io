---
layout: article
title: 1. ggplot2 산점도 with mpg data
permalink: /notes/kr/R/Ch00_ggplot_0
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
mermaid: true
mathjax: true
---


<br>

# visualization R Tibble
<br>

## Tibble?


R에서 tibble은 dataframe을 편리하게 쓸 수 있도록 확장한 버전으로 tidyverse 패키지의 일부다. df랑 상호교환이 가능하다. 콘솔 출력이 좀 더 깔끔하고, 더 편리하고 가독성이 좋다는 특징을 가지고 있다.

tibble에서의 변수 형태

```
– int: integers
– dbl: doubles, real numbers
– chr: character vectors, or strings
– dttm: date-times (a date + a time)
– lgl: logical (TRUE/FALSE)
– fct: factors
– date: dates
```


![tibble](image.png)


<br>

## DataFrame과의 차이?


아래서도 볼 것이지만.. 먼저 간단하게 정리해보자면,


1. 출력: tibble은 콘솔에 출력될 때 상위 10개의 행만을 보여주고, 모든 열을 보여주지 않을 수도 있다. 콘솔이 깔끔하다.


2. 열 이름: tibble은 공백이나 특수 문자가 포함된 열 이름도 가능하다! 예를 들어 `:)` 이런 귀여운 smile도 열 이름으로 지정할 수 있다는 것 


3. 하위 설정: tibble에서는 $ 연산자를 사용해도 부분 매칭(subsetting)이 되지 않기 때문에 정확한 열 이름을 작성해야만 한다.


4. 데이터 유형: tibble은 기본적으로 문자열을 인수로 사용하여 문자열을 자동으로 팩터로 변환하지 않는다.SAS 의 cards 문과 같은 형태의 자료입력도 허용한다.


5. 구조: tibble을 사용하면 tibble::glimpse() 함수를 사용하여 데이터의 구조를 더 깔끔하게 볼 수 있다.

6. 세부 동작: tibble은 행 이름을 강조하지 않거나 새로운 열을 생성할 때 행 길이가 다를 경우 오류를 반환하는 등의 특별한(?) 동작을 한다.


<br>

## tibble 활용기

### as_tibble()

data frame 을 tibble 로 바꿔주는 함수다

```r
library(tidyverse)
as_tibble(iris)
```

![Alt text](image-1.png)


위 tibble형태로 출력한 것을 보면, 데이터프레임과 차이가 있다는 것을 알 수 있다.

- dataframe: 모든 자료를 보여줌

- tibble: 처음 10 줄과 화면에 맞는 만큼의 변수만을 보여주며 변수의 type 도 함께 보여줌


<br>

### printing 차이

