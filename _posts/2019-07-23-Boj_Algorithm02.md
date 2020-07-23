---
layout: post
title: (백준 알고리즘_2529) 부등호
tags:
  - 알고리즘(BOJ)
---

<br>

문제

---

두 종류의 부등호 기호 ‘<’와 ‘>’가 k개 나열된 순서열  A가 있다. 우리는 이 부등호 기호 앞뒤에 서로 다른 한 자릿수 숫자를 넣어서 모든 부등호 관계를 만족시키려고 한다. 예를 들어, 제시된 부등호 순서열 A가 다음과 같다고 하자. 

A =>  < < < > < < > < >

부등호 기호 앞뒤에 넣을 수 있는 숫자는 0부터 9까지의 정수이며 선택된 숫자는 모두 달라야 한다. 아래는 부등호 순서열 A를 만족시키는 한 예이다. 

**3 < 4 < 5 < 6 > 1 < 2 < 8 > 7 < 9 > 0**

이 상황에서 부등호 기호를 제거한 뒤, 숫자를 모두 붙이면 하나의 수를 만들 수 있는데 이 수를 주어진 부등호 관계를 만족시키는 정수라고 한다. 그런데 주어진 부등호 관계를 만족하는 정수는 하나 이상 존재한다. 예를 들어 3456128790 뿐만 아니라 5689023174도 아래와 같이 부등호 관계 A를 만족시킨다. 

**5 < 6 < 8 < 9 > 0 < 2 < 3 > 1 < 7 > 4**

여러분은 제시된 k개의 부등호 순서를 만족하는 (k+1)자리의 정수 중에서 최댓값과 최솟값을 찾아야 한다. 앞서 설명한 대로 각 부등호의 앞뒤에 들어가는 숫자는 { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 }중에서 선택해야 하며 **선택된 숫자는 모두 달라야 한다**. 

<br>

입력

---

첫 줄에 부등호 문자의 개수를 나타내는 정수 k가 주어진다. 그 다음 줄에는 k개의 부등호 기호가 하나의 공백을 두고 한 줄에 모두 제시된다. k의 범위는 2 ≤ k ≤ 9 이다. 

<br>

출력

----

여러분은 제시된 부등호 관계를 만족하는 k+1 자리의 최대, 최소 정수를 첫째 줄과 둘째 줄에 각각 출력해야 한다. 단 아래 예(1)과 같이 첫 자리가 0인 경우도 정수에 포함되어야 한다. 모든 입력에 답은 항상 존재하며 출력 정수는 하나의 문자열이 되도록 해야 한다. 

<br>

풀이

---

부등호를 조건을 만족시키는 n+1길이의 문자열을 재귀호출로 만듬.

가능한 모든 경우의 문자열을 ArrayList에 넣어서 첫번째 값과 마지막 값을 출력함.

```java
import java.io.*;
import java.util.*;

public class Boj2529 {


  static int n;
  static boolean[] visited = new boolean[10];
  static char[] signs;
  static ArrayList<String> list = new ArrayList<>();
	
	public static void main(String[] args) throws NumberFormatException, IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		n = Integer.parseInt(br.readLine());
		signs = new char[n];
    StringTokenizer st = new StringTokenizer(br.readLine());
    for(int i=0; i<n; i++) {
      signs[i] = st.nextToken().charAt(0);
    }
      
    solution("",0);
    System.out.println(list.get(list.size()-1));
    System.out.println(list.get(0)); 
	}
	
	static void solution(String ret, int index) {
		if(index==n+1){
			list.add(ret);
			return;
		}
		
		for(int i=0; i<10; i++) {
			if(visited[i]) continue; 
			if(index==0 || compare(ret.charAt(index-1)-'0',i,signs[index-1])) {
				 StringBuilder sb = new StringBuilder(ret);
				 sb.append(i);
				 visited[i] = true;
				 solution(sb.toString(),index+1);
				 visited[i] = false;
			}
		}
	}
	
	
	static boolean compare(int a, int b, char s) {
		boolean ret = false;
		if(s=='<') {
			if(a<b) ret = true;
		}else {
			if(a>b) ret = true;
		}
		return ret;
	}

}
```
