---
layout: post
title: (백준 알고리즘_10610) 30
tags:
  - 알고리즘(BOJ)
---

<br>

### 문제

---

어느 날, 미르코는 우연히 길거리에서 양수 N을 보았다. 미르코는 30이란 수를 존경하기 때문에, 그는 길거리에서 찾은 수에 포함된 숫자들을 섞어 30의 배수가 되는 가장 큰 수를 만들고 싶어한다.

미르코를 도와 그가 만들고 싶어하는 수를 계산하는 프로그램을 작성하라.

<br>

### 입력

---

N을 입력받는다. N는 최대 105개의 숫자로 구성되어 있으며, 0으로 시작하지 않는다.

<br>

### 출력

---

미르코가 만들고 싶어하는 수가 존재한다면 그 수를 출력하라. 그 수가 존재하지 않는다면, -1을 출력하라.

<br>

### 풀이

---

30의 배수가 되는 수의 조건

- 마지막이 0으로 끝난다.
- 각 자리수의 합이 3의 배수

<br>

```java
public class Boj10610 {

	public static void main(String[] args) throws NumberFormatException, IOException {
		// TODO Auto-generated method stub

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		char[] nArr = br.readLine().toCharArray();
		int[] cntArr = new int[10];
		
		for(int i=0; i<nArr.length; i++) {
		    cntArr[nArr[i]-'0']++;
		}
		
		String ans;
		if(cntArr[0]==0) ans = "-1";
		else {
			int sum = 0;
			StringBuilder sb = new StringBuilder();
			for(int i=cntArr.length-1; i>=0; i--) {
				for(int j=0; j<cntArr[i]; j++) {
					sum += i;
					sb.append(i);
				}
			}
      
			if(sum%3==0) ans = sb.toString();
			else ans = "-1";
		}
		
		System.out.println(ans);

	}

}

```
