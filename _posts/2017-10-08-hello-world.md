---
layout: post
title: [AI 이노베이션스퀘어_기본과정] Day1 수업내용
tags:
  - AI 이노베이션스퀘어 기본과정
---

```python
import sys
sys.version
```

```
'3.7.6 (default, Jan  8 2020, 13:42:34) \n[Clang 4.0.1 (tags/RELEASE_401/final)]'
```

<br>

```python
a=10
b=20
c=a+b
c
```

```
30
```

<br>

### 노트북 단축키 요약

- shift + enter : 셀 실행 후 다음 셀로 이동
- alt + enter : 셀 실행 후 다음에 셀을 추가
- Tab : 들여쓰기
- shift + Tab : 들여쓰기 해제
- ctrl + '/' : 주석문 설정/해제
- ESC -> 'm' : mark down

<br>

### 정수형

```python
#파이썬 자료형
a=100
b=10
print(type(a))
print(int(a))
print(bin(b))
```

```
<class 'int'>
100
0b1010
```

<br>

### 실수형 : float

```python
f = 10.54
print(f)
b = float(a) #형변환
print(b,type(b))
```

```
10.54
10.0 <class 'float'>
```

<br>

### 객체 : 파이썬의 모든 변수, 데이터형은 객체로 관리된다

```python
a=30  #int형 클래스의 인스턴스 객체 생성
print(hex(id(a)))
b=40  #int형 클래스의 인스턴스 객체 생성
print(hex(id(b)))
b=a   #참조값 변경
print(hex(id(b)))
b=50  #int형 클래스의 인스턴스 객체 생성
print(hex(id(b)))

a = 1000 #int형 클래스의 인스턴스 객체 생성
print("a",hex(id(a)))

del b
#print(hex(id(b)))
```

```
0x10337b820
0x10337b960
0x10337b820
0x10337baa0
a 0x105a02990
```

<br>

```python
#### 사칙연산
# // 정수 나누기
# ** 제곱

a=10
b=3
f = a/b
ff = a//b
print(ff,type(ff))
print(f,type(f))

h = a**b  #제곱
print(h)
```

```
3 <class 'int'>
3.3333333333333335 <class 'float'>
1000
```

<br>

```python
type(10.)
```

```
float
```

<br>

### 표준입출력

#표준 입력 : 변수=input( )

```python
a = input()
print(a)
print(type(a))
```

```
100
100
<class 'str'>
```

<br>

```python
math = input("수학점수")
english = input("영어점수")
total = int(math)+int(english)
print(total)


```

```
수학점수100
영어점수100
200

```

<br>

### 표준출력 : print()

```python
print('a:',4,'b:',5)

```

```
a: 4 b: 5

```

<br>

```python
#end
print(1,2,end='    ')
print(3,4)

```

```
1 2    3 4

```

<br>

```python
#sep
print(1,2,3,4,5,sep=" & ")

```

```
1 & 2 & 3 & 4 & 5

```

<br>

### print formatting

```python
num_1 = 10
num_2 = 1.234
str_1 = "Hello python"

print("int형 : %d"%num_1)
print("float형 : %f"%num_2)
print("문자열 %s"%str_1)
print("16진수 0x%x"%num_1)

print("%s %d"%(str_1,10000))

```

```
int형 : 10
float형 : 1.234000
문자열 Hello python
16진수 0xa
Hello python 10000

```

<br>

```python
#format() 함수 사용

print(format(1.23456,"7.6f"))  #전체 7자리 소숫점은 6자리로 표현

#
print('{} + {}' .format("math","eng"))
print('{:*<10} : {:>5}'.format("dahye",100))
print('{:*<10} : {:>5}'.format("dahyeJung",1))
print('{:*<10} : {:>5}'.format("amy",152))
print('{:!^10}'.format("hello"))

```

```
1.234560
math + eng
dahye***** :   100
dahyeJung* :     1
amy******* :   152
!!hello!!!

```

<br>

### eval( ) 함수 : 문자열로 표현된 파이썬 식을 인자로 받아 인터프리터가 번역하여 실행

```python
a=10
express = 'a'+'+'+'20'
print(express)
b = eval(express)
print(b)

```

```
a+20
30

```

