---
layout: post
title: (백준 알고리즘_1202) 보석도둑
tags:
  - 알고리즘(BOJ)
---

<br>

### 문제

---

세계적인 도둑 상덕이는 보석점을 털기로 결심했다.

상덕이가 털 보석점에는 보석이 총 N개 있다. 각 보석은 무게 Mi와 가격 Vi를 가지고 있다. 상덕이는 가방을 K개 가지고 있고, 각 가방에 담을 수 있는 최대 무게는 Ci이다. 가방에는 최대 한 개의 보석만 넣을 수 있다.

상덕이가 훔칠 수 있는 보석의 최대 가격을 구하는 프로그램을 작성하시오.

<br>

### 입력

---

첫째 줄에 N과 K가 주어진다. (1 ≤ N, K ≤ 300,000)

다음 N개 줄에는 각 보석의 정보 Mi와 Vi가 주어진다. (0 ≤ Mi, Vi ≤ 1,000,000)

다음 K개 줄에는 가방에 담을 수 있는 최대 무게 Ci가 주어진다. (1 ≤ Ci ≤ 100,000,000)

모든 숫자는 양의 정수이다.

<br>

### 출력

---

첫째 줄에 상덕이가 훔칠 수 있는 보석 가격의 합의 최댓값을 출력한다.

<br>

### 예제 입력과 출력

---

```java
#입력
3 2
1 65
5 23
2 99
10
2
```

```java
#출력
164
```

<br>

### 풀이

---

모든 보석과 가방의 정보를 ArrayList에 받아서 무게 오름차순으로 정렬하였다. 이 ArrayList를 for문으로 돌면서 <b>보석</b>을 만나면 보석의 가격 정보를 우선순위큐에 넣었고, <b>가방</b>을 만나면 우선순위큐에서 값을 꺼내 가격을 더할 변수에 더했다.

주의할 점 : 

- 입력 조건을 고려했을 때  훔칠 수 있는 보석 가격의 합의 최댓값은 3*10^11이 나오기 때문에 가격을 더할 변수는 long으로 선언해야 한다.
- 자바 PriorityQueue의 default는 minHeap이다. 이 예제에서는 우선순위큐에서 가장 비싼 보석을 꺼내야하기 때문에, 우선순위큐에 값을 넣을 때와 꺼낼 때 모두 -1을 곱함.

```java
public class Boj1202 {

	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n = Integer.parseInt(st.nextToken());
		int k = Integer.parseInt(st.nextToken());
		ArrayList<Jewelry> list = new ArrayList<>();
		
		for(int i=0; i<n; i++) {
			st = new StringTokenizer(br.readLine());
			int m = Integer.parseInt(st.nextToken());
			int v = Integer.parseInt(st.nextToken());
			list.add(new Jewelry(m,v));
		}
		
		int[] bags = new int[k];
		for(int i=0; i<k; i++) {
			bags[i] = Integer.parseInt(br.readLine());
			list.add(new Jewelry(bags[i],-1));  //가방의 경우 무게만 입력하고,가격은 -1로 해놓음.
		}
		
		Collections.sort(list,(a,b)->a.m!=b.m?a.m-b.m:b.v-a.v);  //무게오름차순.가격내림차순
		
		PriorityQueue<Integer> pq = new PriorityQueue<>();
		long sum = 0;
		for(Jewelry j : list) {
			if(j.v==-1) {
				if(!pq.isEmpty()) sum += (-pq.poll());
			}else{
				pq.add(-j.v);
			}
		}
		
		System.out.println(sum);
		
	}

}

class Jewelry{
	int m;
	int v;
	Jewelry(int m, int v){
		this.m = m;
		this.v =v;
	}
}
```
