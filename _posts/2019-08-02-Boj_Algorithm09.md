---
layout: post
title: (백준 알고리즘_2109) 순회강연
tags:
  - 알고리즘(BOJ)
---

<br>

### 문제

---

한 저명한 학자에게 n(0≤n≤10,000)개의 대학에서 강연 요청을 해 왔다. 각 대학에서는 d(1≤d≤10,000)일 안에 와서 강연을 해 주면 p(1≤p≤10,000)만큼의 강연료를 지불하겠다고 알려왔다. 각 대학에서 제시하는 d와 p값은 서로 다를 수도 있다. 이 학자는 이를 바탕으로, 가장 많은 돈을 벌 수 있도록 순회강연을 하려 한다. 강연의 특성상, 이 학자는 하루에 최대 한 곳에서만 강연을 할 수 있다.

예를 들어 네 대학에서 제시한 p값이 각각 50, 10, 20, 30이고, d값이 차례로 2, 1, 2, 1 이라고 하자. 이럴 때에는 첫째 날에 4번 대학에서 강연을 하고, 둘째 날에 1번 대학에서 강연을 하면 80만큼의 돈을 벌 수 있다.

<br>

### 입력

---

첫째 줄에 정수 n이 주어진다. 다음 n개의 줄에는 각 대학에서 제시한 p값과 d값이 주어진다.

<br>

### 출력

---

첫째 줄에 최대로 벌 수 있는 돈을 출력한다.

<br>

### 예제 입력과 출력

---

```java
#입력
7
20 1
2 1
10 3
100 2
8 2
5 20
50 10
```

```java
#출력
185
```

<br>

### 풀이

---

앞에서 풀었던 보석도둑과 거의 비슷한 방법으로 해결.

```java
public class Boj2109 {

	public static void main(String[] args) throws NumberFormatException, IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		ArrayList<Lecture> list = new ArrayList<>();
		int maxD = 0;
		for(int i=0; i<n; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			int pay = Integer.parseInt(st.nextToken()); //p 
			int day = Integer.parseInt(st.nextToken()); //d
			list.add(new Lecture(pay,day));
			maxD = Math.max(maxD,day);
		}
		
		for(int i=1; i<=maxD; i++) {
			list.add(new Lecture(0,i));
		}
		
		Collections.sort(list,(a,b)->a.day!=b.day?b.day-a.day:b.pay-a.pay);		
    
		PriorityQueue<Integer> pq = new PriorityQueue<>();	
		int sum = 0;
		for(Lecture lec : list) {
			if(lec.pay==0) { 
				if(!pq.isEmpty()) sum += (-pq.poll());
			}else {
				pq.add(-lec.pay);
			}
		}
				
		System.out.println(sum);
		
		
	}

}

class Lecture{
	int pay;
	int day;
	Lecture(int pay, int day){
		this.pay = pay;
		this.day = day;
	}
}
```
