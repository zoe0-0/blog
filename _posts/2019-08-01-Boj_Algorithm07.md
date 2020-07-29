---
layout: post
title: (백준 알고리즘_10815) 숫자카드
tags:
  - 알고리즘(BOJ)
---

<br>

### 문제

---

숫자 카드는 정수 하나가 적혀져 있는 카드이다. 상근이는 숫자 카드 N개를 가지고 있다. 정수 M개가 주어졌을 때, 이 수가 적혀있는 숫자 카드를 상근이가 가지고 있는지 아닌지를 구하는 프로그램을 작성하시오.

<br>

### 입력

---

첫째 줄에 상근이가 가지고 있는 숫자 카드의 개수 N(1 ≤ N ≤ 500,000)이 주어진다. 둘째 줄에는 숫자 카드에 적혀있는 정수가 주어진다. 숫자 카드에 적혀있는 수는 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다. 두 숫자 카드에 같은 수가 적혀있는 경우는 없다.

셋째 줄에는 M(1 ≤ M ≤ 500,000)이 주어진다. 넷째 줄에는 상근이가 가지고 있는 숫자 카드인지 아닌지를 구해야 할 M개의 정수가 주어지며, 이 수는 공백으로 구분되어져 있다. 이 수도 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다

<br>

### 출력

---

첫째 줄에 입력으로 주어진 M개의 수에 대해서, 각 수가 적힌 숫자 카드를 상근이가 가지고 있으면 1을, 아니면 0을 공백으로 구분해 출력한다.

<br>

### 예제 입력과 출력

---

```java
#입력
5
6 3 2 10 -10
8
10 9 -5 2 3 4 5 -10
```

```java
#출력
1 0 0 1 1 0 0 1
```

<br>

### 풀이

---

M개의 정수가 N개의 숫자카드에 있는지 확인하기 위해 이중 for문으로 접근한다면, 	`M(1 ≤ M ≤ 500,000)`  ,` N(1 ≤ N ≤ 500,000)` 이기 때문에 최대 (500,000 * 20,000,000) 번 탐색을 하게 된다. 이분탐색을 활용하면 더 빠르게 답을 찾을 수 있다. 

```java
public class Boj10815 {
	
	static int[] arr;
	
	public static void main(String[] args) throws NumberFormatException, IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		arr = new int[n];
		StringTokenizer st = new StringTokenizer(br.readLine());
		for(int i=0; i<n; i++) {
			arr[i] = Integer.parseInt(st.nextToken());
		}
		Arrays.sort(arr);
		
		int m = Integer.parseInt(br.readLine());
		st = new StringTokenizer(br.readLine());
		StringBuilder sb = new StringBuilder();
    
		for(int i=0; i<m; i++) {
			int num = Integer.parseInt(st.nextToken());
			sb.append(binarySearch(0,n-1,num));
			sb.append(" ");
		}
		System.out.println(sb.toString());
	}
	
	static int binarySearch(int left,int right,int target) {
		while(left<=right) {
			int mid = (left+right)/2;
			if(arr[mid]==target) return 1;
			else if(arr[mid]<target) left = mid+1;
			else right = mid-1;
		}
		return 0;
	}

}

```

