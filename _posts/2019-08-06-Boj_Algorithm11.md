---
layout: post
title: (백준 알고리즘_2875) 대회 or 인턴
tags:
  - 알고리즘(BOJ)
---

<br>

### 문제

---

백준대학교에서는 대회에 나갈 때 2명의 여학생과 1명의 남학생이 팀을 결성해서 나가는 것이 원칙이다. (왜인지는 총장님께 여쭈어보는 것이 좋겠다.)

백준대학교는 뛰어난 인재들이 많아 올해에도 N명의 여학생과 M명의 남학생이 팀원을 찾고 있다. 대회에 참여하려는 학생들 중 K명은 반드시 인턴쉽 프로그램에 참여해야 한다. 인턴쉽에 참여하는 학생은 대회에 참여하지 못한다.

백준대학교에서는 뛰어난 인재들이 많기 때문에, 많은 팀을 만드는 것이 최선이다.

여러분은 여학생의 수 N, 남학생의 수 M, 인턴쉽에 참여해야하는 인원 K가 주어질 때 만들 수 있는 최대의 팀 수를 구하면 된다.

<br>

### 입력

---

첫째 줄에 N, M, K가 순서대로 주어진다. (0 ≤ M ≤ 100, 0 ≤ N ≤ 100, 0 ≤ K ≤ M+N)

<br>

### 출력

---

만들 수 있는 팀의 최대 개수을 출력하면 된다.

<br>

### 예제 입력과 출력

---

```java
#입력
6 3 2
```

```java
#출력
2
```

<br>

### 풀이

---

m과 n/2 중에서 더 작은 수가 현재 상태에서 만들 수 있는 팀의 최대 갯수이다. => `int team = Math.min(m,n/2);`

m에서 팀을 만들고 남은 인원수는 `m-team` 이고, n에서 팀을 만들고 남은 인원수는 `n-2*team` 이다

따라서 팀을 만들지 못한 전체 인원수는 `m+n-3*team` 이다.

만들 수 있는 팀의 최대 개수를 구하기 위해서는, 먼저 팀을 만들지 못한 인원들을 인턴쉽에 참여하게 한다. => `k -= m+n-3*team;`

그래도 인턴쉽에 참여해야 하는 인원을 채우지 못했다면 (`k>0`),  만들어진 팀을 쪼개서 인턴쉽에 참여하게 한다.

<br>

```java
public class Boj2875 {

	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n = Integer.parseInt(st.nextToken());
		int m = Integer.parseInt(st.nextToken());
		int k = Integer.parseInt(st.nextToken());
		
		int team = Math.min(m,n/2);
		k -= m+n-3*team;
		
		while(k>0) {
		  k -= 3;
		  team--;
		}
		
		System.out.println(team);
		
	}

}

```
