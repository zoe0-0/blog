---
layout: post
title: (백준 알고리즘_11729) 하노이 탑 이동 순서
tags:
  - 알고리즘(BOJ)
---

<br>

### 문제

---

세 개의 장대가 있고 첫 번째 장대에는 반경이 서로 다른 n개의 원판이 쌓여 있다. 각 원판은 반경이 큰 순서대로 쌓여있다. 이제 수도승들이 다음 규칙에 따라 첫 번째 장대에서 세 번째 장대로 옮기려 한다.

1. 한 번에 한 개의 원판만을 다른 탑으로 옮길 수 있다.
2. 쌓아 놓은 원판은 항상 위의 것이 아래의 것보다 작아야 한다.

이 작업을 수행하는데 필요한 이동 순서를 출력하는 프로그램을 작성하라. 단, 이동 횟수는 최소가 되어야 한다.

아래 그림은 원판이 5개인 경우의 예시이다.

![img](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/11729/hanoi.png)

<br>

### 입력

---

첫째 줄에 첫 번째 장대에 쌓인 원판의 개수 N (1 ≤ N ≤ 20)이 주어진다.

<br>

### 출력

---

첫째 줄에 옮긴 횟수 K를 출력한다.

두 번째 줄부터 수행 과정을 출력한다. 두 번째 줄부터 K개의 줄에 걸쳐 두 정수 A B를 빈칸을 사이에 두고 출력하는데, 이는 A번째 탑의 가장 위에 있는 원판을 B번째 탑의 가장 위로 옮긴다는 뜻이다.

<br>

### 예제 입력과 출력

---

```java
#입력
3 
```

```java
#출력
7
1 3
1 2
3 2
1 3
2 1
2 3
1 3
```

<br>

### 풀이

---

주의할 점 : StringBuilder를 사용하지 않고 매번 `System.out.println(x+" "+y);`  출력했더니 시간초과.

StringBuilder로 출력 값을 append하고 최종적으로 한번 출력해주니까 통과

```java
public class Boj11729 {

	static int count = 0;
	static StringBuilder sb = new StringBuilder();
	
	public static void main(String[] args) throws NumberFormatException, IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		solve(n,1,3);
		System.out.println(count);
		System.out.println(sb.toString());
		
	}
	
	//1부터 n까지의 원판은 x에서 y로 이동하는 방법
	static void solve(int n,int x,int y) {
		
		if(n==0) return;
		int z = 6-(x+y);
      
	    solve(n-1,x,z); //1~n-1까지의 원판을 x에서 z로 이동
	    
	    sb.append(x+" "+y+"\n");  //n번째 원판을 x에서 y로 이동
      //System.out.println(x+" "+y);  시간초과
	    ++count;
	    
      solve(n-1,z,y);  	//1~n-1까지의 원판을 z에서 y로 이동


	}	
}
```
