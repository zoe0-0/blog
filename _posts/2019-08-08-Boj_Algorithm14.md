---
layout: post
title: (백준 알고리즘_12970) AB
tags:
  - 알고리즘(BOJ)
---

<br>

### 문제

---

정수 N과 K가 주어졌을 때, 다음 두 조건을 만족하는 문자열 S를 찾는 프로그램을 작성하시오.

- 문자열 S의 길이는 N이고, 'A', 'B'로 이루어져 있다.
- 문자열 S에는 0 ≤ i < j < N 이면서 s[i] == 'A' && s[j] == 'B'를 만족하는 (i, j) 쌍이 K개가 있다.

<br>

### 입력

---

첫째 줄에 N과 K가 주어진다. (2 ≤ N ≤ 50, 0 ≤ K ≤ N(N-1)/2)

<br>

### 출력

---

첫째 줄에 문제의 조건을 만족하는 문자열 S를 출력한다. 가능한 S가 여러 가지라면, 아무거나 출력한다. 만약, 그러한 S가 존재하지 않는 경우에는 -1을 출력한다.

<br>

### 풀이

---

- a(A의 갯수) + b(B의 갯수) = N (문자열의 길이)일때, 조건을 만족하는 쌍의 갯수는 0 ~ a*b개
- 먼저 문자열의 길이 N만큼 B로 채운다
- a+b=N을 만족하는 a와 b를 구하면서,  a*b와 k를 대소비교한다
- 조건을 만족하는 쌍의 갯수는 최소 0개에서 최대 a*b*개이기 때문에, k는 a*b보다 같거나 작아야만 한다
- a*b==k일 경우에는 N길이만큼 B로 채운 문자열의 앞에서부터 a만큼 A로 채우면 된다.
- a*b>k일 경우에는, N길이만큼 B로 채운 문자열의 앞에서부터 a-1만큼 A로 채운 후에 나머지 A는 문자열의 맨 마지막에 넣어준다. 이 경우 조건을 만족하는 쌍의 갯수는 (a-1)*b 개 이다. 문제에서 주어진 조건을 만족하는 쌍의 갯수는 k이기 때문에  `k - (a-1)*b`의 쌍이 추가적으로 필요하다.  따라서 맨 뒤에 있는 A를 앞으로  `k - (a-1)*b` 만큼 이동한다. 

<br>

```java
public class Main {

	static StringBuilder sb = new StringBuilder();
	
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n = Integer.parseInt(st.nextToken());
		int k = Integer.parseInt(st.nextToken());
	
		//길이 n만큼 B로 채운다
		for(int i=0; i<n; i++) sb.append("B");
		
		if(k==0) { 
			sb.replace(n-1, n, "A");
		}else{
			for(int i=0; i<=n; i++) {
				int a = i;
				int b = n-i;
				if(a*b<k) continue;  
				
				if(a*b==k) {
				    fillA(a);
				}else {  
					fillA(a-1);
					int tmp = k-((a-1)*b);
					sb.replace(n-1-tmp, n-tmp, "A");
				}
				break;	
			}
		}

		String str = sb.toString();
		if(!str.contains("A")) System.out.println(-1);
		else System.out.println(str);
		
		
	}
	
	static void fillA(int endIndex) {
		for(int i=0; i<endIndex; i++) {
			sb.replace(i, i+1, "A");
		}
	}
		
}
```

