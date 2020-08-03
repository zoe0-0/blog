---
layout: post
title: (백준 알고리즘_12904) A와 B
tags
  - 알고리즘(BOJ)
---

<br>

### 문제

---

수빈이는 A와 B로만 이루어진 영어 단어가 존재한다는 사실에 놀랐다. 대표적인 예로 AB (Abdominal의 약자), BAA (양의 울음 소리), AA (용암의 종류), ABBA (스웨덴 팝 그룹)이 있다.

이런 사실에 놀란 수빈이는 간단한 게임을 만들기로 했다. 두 문자열 S와 T가 주어졌을 때, S를 T로 바꾸는 게임이다. 문자열을 바꿀 때는 다음과 같은 두 가지 연산만 가능하다.

- 문자열의 뒤에 A를 추가한다.
- 문자열을 뒤집고 뒤에 B를 추가한다.

주어진 조건을 이용해서 S를 T로 만들 수 있는지 없는지 알아내는 프로그램을 작성하시오. 

<br>

### 입력

---

첫째 줄에 S가 둘째 줄에 T가 주어진다. (1 ≤ S의 길이 ≤ 999, 2 ≤ T의 길이 ≤ 1000, S의 길이 < T의 길이)

<br>

### 출력

---

S를 T로 바꿀 수 있으면 1을 없으면 0을 출력한다.

<br>

### 예제 입력과 출력

---

```java
#입력
B
ABBA
```

```java
#출력
1
```

<br>

### 풀이

---

문자열을 바꾸는 규칙을 적용하면 문자열 맨 마지막 문자가 <b>A</b>로 끝나거나 <b>B</b>로 끝나는 경우만 존재한다.

그렇다면 규칙을 적용하여 바뀐 문자열에서 원래 문자열로 돌아가는 방법을 살펴보자.

A로 끝나는 문자열의 경우 마지막 문자인 A를 제거하면 되고, B로 끝나는 문자열은 마지막 문자인 B를 제거하고 문자열을 뒤집으면 된다.

주어진 입력 T에서 마지막 문자부터 역순으로 이 과정을 통해 원래 문자열로 복원하는 과정을 거친다. 복원된 문자열이 입력에 주어진 S와 같다면 1을 출력하고, 다르다면 0을 출력한다.

<br>

```java
public class Boj12904 {

	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String start = br.readLine();
		String end = br.readLine();
		StringBuilder sb = new StringBuilder(end);
		
		for(int i=end.length()-1; i>=start.length(); i--) {
			char lastChar = sb.charAt(sb.length()-1); //현재 문자열의 마지막 문자
			sb.deleteCharAt(sb.length()-1); //마지막 문자를 제거한다.
			if(lastChar=='B') {  //마지막 문자가 'B'였을 경우=> 문자열 뒤집기
				sb.reverse();
			}
		}
		
		if(sb.toString().equals(start)) System.out.println(1);
		else System.out.println(0);
		
	}

}

```

