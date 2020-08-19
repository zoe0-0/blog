---
layout: post
title: (백준 알고리즘_1715) 카드 정렬하기
tags:
  - 알고리즘(BOJ)
---

<br>

### [문제](https://www.acmicpc.net/problem/1715)

---

1. 정렬된 두 묶음의 숫자 카드가 있다고 하자. 각 묶음의 카드의 수를 A, B라 하면 보통 두 묶음을 합쳐서 하나로 만드는 데에는 A+B 번의 비교를 해야 한다. 이를테면, 20장의 숫자 카드 묶음과 30장의 숫자 카드 묶음을 합치려면 50번의 비교가 필요하다.

   매우 많은 숫자 카드 묶음이 책상 위에 놓여 있다. 이들을 두 묶음씩 골라 서로 합쳐나간다면, 고르는 순서에 따라서 비교 횟수가 매우 달라진다. 예를 들어 10장, 20장, 40장의 묶음이 있다면 10장과 20장을 합친 뒤, 합친 30장 묶음과 40장을 합친다면 (10+20)+(30+40) = 100번의 비교가 필요하다. 그러나 10장과 40장을 합친 뒤, 합친 50장 묶음과 20장을 합친다면 (10+40)+(50+20) = 120 번의 비교가 필요하므로 덜 효율적인 방법이다.

   N개의 숫자 카드 묶음의 각각의 크기가 주어질 때, 최소한 몇 번의 비교가 필요한지를 구하는 프로그램을 작성하시오.

<br>

### 입력

---

첫째 줄에 N이 주어진다. (1≤N≤100,000) 이어서 N개의 줄에 걸쳐 숫자 카드 묶음의 각각의 크기가 주어진다.

<br>

### 출력

---

첫째 줄에 최소 비교 횟수를 출력한다. (21억 이하)

<br>

### 예제 입력과 출력

---

```java
//입력
3
10
20
40
```

```java
//출력
100
```

<br>

### 풀이

---

- 10,20,40 3개의 카드 묶음이 있다고 할 때,
  - (10+20)+(30+40) = 100번 비교
  - (10+40)+(50+20) = 120번 비교 
- 즉, 카드의 수가 작은 묶음끼리 먼저 합쳐나갈 때 가장 작은 비교 횟수를 구할 수 있다.
- 이를 구현하기 위해 자료구조 `Priority Queue` 를 사용. 우선순위큐(min heap)에서는 가장 작은 수부터 꺼내쓸 수 있다.
- 먼저, 우선순위큐에서 카드의 수가 작은 두 개의 카드 묶음을 꺼내서 합친 후 그 값을 `total` 변수에 더한다. 
- 그리고 그 합친 값을 다시 큐에 넣어야 한다. (왜냐하면, 앞에서 두개의 카드 묶음을 합쳐서 하나로 만들어진 묶음도 또 다른 카드 묶음과 합쳐질때 값이 누적으로 또 더해지기 때문에)

<br>

```java
public class Main {

	public static void main(String[] args) throws NumberFormatException, IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		PriorityQueue<Integer> pq = new PriorityQueue<>();
		for(int i=0; i<n; i++) {
			pq.add(Integer.parseInt(br.readLine()));
		}
		
		int total = 0;
		while(pq.size()!=1) {
			int a = pq.poll();
			int b = pq.poll();
			int sum = a+b;
			total += sum;
			pq.add(sum);
		}
		
		System.out.println(total);
		
	}


}

```



