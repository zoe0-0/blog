---
layout: post
title: (백준 알고리즘_1753) 카드 정렬하기
tags:
  - 알고리즘(BOJ)
---

<br>

### [문제](https://www.acmicpc.net/problem/1753)

---

방향그래프가 주어지면 주어진 시작점에서 다른 모든 정점으로의 최단 경로를 구하는 프로그램을 작성하시오. 단, 모든 간선의 가중치는 10 이하의 자연수이다.

<br>

### 입력

---

첫째 줄에 정점의 개수 V와 간선의 개수 E가 주어진다. (1≤V≤20,000, 1≤E≤300,000) 모든 정점에는 1부터 V까지 번호가 매겨져 있다고 가정한다. 둘째 줄에는 시작 정점의 번호 K(1≤K≤V)가 주어진다. 셋째 줄부터 E개의 줄에 걸쳐 각 간선을 나타내는 세 개의 정수 (u, v, w)가 순서대로 주어진다. 이는 u에서 v로 가는 가중치 w인 간선이 존재한다는 뜻이다. u와 v는 서로 다르며 w는 10 이하의 자연수이다. 서로 다른 두 정점 사이에 여러 개의 간선이 존재할 수도 있음에 유의한다.

<br>

### 출력

---

첫째 줄부터 V개의 줄에 걸쳐, i번째 줄에 i번 정점으로의 최단 경로의 경로값을 출력한다. 시작점 자신은 0으로 출력하고, 경로가 존재하지 않는 경우에는 INF를 출력하면 된다.

<br>

### 예제 입력과 출력

---

```java
//입력
5 6
1
5 1 1
1 2 2
1 3 3
2 3 4
2 4 5
3 4 6
```

```java
//출력
0
2
3
7
INF
```

<br>

### 풀이

---

- 한 지점에서 다른 모든 지점까지의 최단 경로 계산, 양의 간선 => 다익스트라 알고리즘으로 해결
- 방문하지 않은 노드 중에서 최단 거리값을 가지는 노드를 매번 선형으로 탐색한다면 전체 시간 복잡도는 O(V^2)
- 따라서 우선순위 큐(min heap)를 사용해서 문제를 해결한다
- 따로 boolean 배열을 만들어서 visited를 체크하지 않아도 된다
  - 우선순위 큐에서 꺼낸 원소의 거리값 > 현재 테이블에 기록되어있는 거리값 
  - 이미 방문처리가 된 노드로 간주하고 다음 스텝으로 넘어간다 (continue)

<br>

```java
public class Main {
	
	static final int INF = 10000000;

	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int v = Integer.parseInt(st.nextToken());
		int e = Integer.parseInt(st.nextToken());
		int start = Integer.parseInt(br.readLine());
		List<Node>[] graph = new ArrayList[v+1];
		int[] distance = new int[v+1];
		Arrays.fill(distance, INF);
    
		for(int i=0; i<e; i++) {
        st = new StringTokenizer(br.readLine());
        int node1 = Integer.parseInt(st.nextToken());
        int node2 = Integer.parseInt(st.nextToken());
        int weight = Integer.parseInt(st.nextToken());
        if(graph[node1]==null) graph[node1] = new ArrayList<>();
        graph[node1].add(new Node(node2,weight));	
		}
		
		PriorityQueue<Node> pq = new PriorityQueue<>();
		pq.add(new Node(start,0));
			
		while(!pq.isEmpty()) {
        Node cur = pq.poll();
        if(cur.weight >= distance[cur.node]) continue;
        distance[cur.node] = cur.weight; 

        if(graph[cur.node]==null) continue;
        for(Node adj : graph[cur.node]) {  //cur node에 연결된 노드들
            if(distance[adj.node] > distance[cur.node] + adj.weight) {
               pq.add(new Node(adj.node,distance[cur.node] + adj.weight));
            }
        }
		}
		
		
		StringBuilder sb = new StringBuilder();
		for(int i=1; i<v+1; i++) {
		  if(distance[i]!=INF) sb.append(distance[i]+"\n");	
		  else sb.append("INF\n");
		}
		
		System.out.println(sb.toString());
		
	}
}


class Node implements Comparable<Node>{
	int node;
	int weight;
	Node(int node, int weight){
		this.node = node;
		this.weight = weight;
	}
	
	@Override
	public int compareTo(Node o) {
		// TODO Auto-generated method stub
		return Integer.compare(this.weight, o.weight);  //가중치 오름차순
	}
}
```



