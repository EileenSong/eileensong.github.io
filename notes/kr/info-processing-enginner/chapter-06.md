---
layout: article
title: 6. 프로그래밍 언어 활용
permalink: /notes/kr/info-processing-engineer/chapter-06
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
---

## 기본 문법 활용하기
### 프로그래밍을 위한 기본 사항 :star::star:

* **아스키 코드**

    | 10진수 | 부호 | 10진수 | 부호 | 10진수 | 부호 |
    | --- | --- | --- | --- | --- | --- |
    | 0 | NULL | 65 | A | 97 | a |
    | 32 | ‘ ‘(Space) | 66 | B | 98 | b |
    | 48 | 0 | 67 | C | 99 | c |
    | 49 | 1 | 68 | D | 100 | d |

### 변수 활용 :star::star::star:

* **식별자 표기법:** 카멜 표기법, 파스칼 표기법, 스네이크 표기법, 헝가리안 표기법

### 연산자

* **연산자 우선순위:** **증**감 연산자, **산**술 연산자, **시**프트 연산자, **관**계 연산자, **비**트 연산자, **논**리 연산자, **삼**항 연산자, **대**입 연산자   `#증산시 관비 논삼대`{:.info}

### 기출 문제 :star::star::star::star::star:

``` c
#include <stdio.h>
void main() {
	// Bubble Sort
	int i, j;
	int temp;
	int a[5] = {75, 95, 85, 100, 50};

	for(i = 0; i < 4; i++) {
		for(j = 0; j < 4 - i) {
			if(a[j] > a[j + 1]) {
				temp = a[j];
				a[j] = a[j + 1];
				a[j + 1] = temp;
			}
		}
	}

	for(i = 0; i < 4; i++) {
		printf("%d", a[i]);
	}
}
```
<span style="color: grey; font-size: 12px;">정답: 50 75 85 95 100</span>
    

``` java
public static void main(String[] args) {
	int i;
	int[] a = {0, 1, 2, 3};

	for(i = 0; i < 4; i++) {
		System.out.print(a[i] + " ");
	} 
}
```
<span style="color: grey; font-size: 12px;">정답: 0 1 2 3</span>
    

``` java
public static void main(String[] args) {
	int i = 3;
	int k = 1;

	switch(i) {
		case 0:
		case 1:
		case 2:
		case 3: k = 0;
		case 4: k += 3;
		case 5: k -= 10;
		default: k--;
	}

	System.out.print(k);
}
```
<span style="color: grey; font-size: 12px;">정답: -8</span>

``` python
a = {'일본', '중국', '한국'}
a.add('베트남')
a.add('중국')
a.remove('일본')
a.update({'홍콩', '한국', '태국'})
print(a)
```
<span style="color: grey; font-size: 12px;">정답: {’중국’, ‘한국’, ‘베트남’, ‘홍콩’, ‘태국’ }</span>

``` java
class Parent {
	public void show() {
		System.out.println("Parent");
	}
}

class Child {
	public void show() {
		System.out.println("Child");
	}
}

public class Main {
	public static void main(String[] args) {
		Parent pa = ___ Child();
		pa.show():
	}
}
```
<span style="color: grey; font-size: 12px;">
정답: 빈칸 = new, 결과 = Child
</span>

``` java
class A {
	private int a;

	public A(int a) {
		this.a = a;
	}

	public void display() {
		Syetem.out.println("a=" + a);
	}
}

class B extend A {
	public B(int a) {
		super(a);
		super.display();
	}
}

public class Main {
	public static void main(String[] args) {
		B obj = new B(10);
	}
}
```
<span style="color: grey; font-size: 12px;">정답: 10</span>
    

``` c
#include <stdio.h>
void main() {
	int i = 0, c = 0;

	while(i < 10) {
		i++;
		c *= i;
	}

	printf("%d", c);
}
```
<span style="color: grey; font-size: 12px;">정답: 0</span>

``` c
#include <stdio.h>
int r1() {
	return 4;
}

int r10() {
	return (30 + r1());
}

int r100() {
	return (200 + r10());
}

int main() {
	printf("%d", r100());
	return 0;
}
```
<span style="color: grey; font-size: 12px;">정답: 234</span>

``` java
public static void main(String[] args) {
	int i = 0;
	int sum = 0;

	while(i < 10) {
		i++;
		if(i % 2 == 1) continue;
		sum += i;
	}
	System.out.println(sum);
}
```
<span style="color: grey; font-size: 12px;">정답: 30</span>