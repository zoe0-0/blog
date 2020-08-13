---
layout: post
title: (백준 알고리즘_3190) 뱀
tags:
  - 알고리즘(BOJ)
---

<br>

### 문제

---

 'Dummy' 라는 도스게임이 있다. 이 게임에는 뱀이 나와서 기어다니는데, 사과를 먹으면 뱀 길이가 늘어난다. 뱀이 이리저리 기어다니다가 벽 또는 자기자신의 몸과 부딪히면 게임이 끝난다.

게임은 NxN 정사각 보드위에서 진행되고, 몇몇 칸에는 사과가 놓여져 있다. 보드의 상하좌우 끝에 벽이 있다. 게임이 시작할때 뱀은 맨위 맨좌측에 위치하고 뱀의 길이는 1 이다. 뱀은 처음에 오른쪽을 향한다.

뱀은 매 초마다 이동을 하는데 다음과 같은 규칙을 따른다.

- 먼저 뱀은 몸길이를 늘려 머리를 다음칸에 위치시킨다.
- 만약 이동한 칸에 사과가 있다면, 그 칸에 있던 사과가 없어지고 꼬리는 움직이지 않는다.
- 만약 이동한 칸에 사과가 없다면, 몸길이를 줄여서 꼬리가 위치한 칸을 비워준다. 즉, 몸길이는 변하지 않는다.

사과의 위치와 뱀의 이동경로가 주어질 때 이 게임이 몇 초에 끝나는지 계산하라.

<br>

### 입력

---

첫째 줄에 보드의 크기 N이 주어진다. (2 ≤ N ≤ 100) 다음 줄에 사과의 개수 K가 주어진다. (0 ≤ K ≤ 100)

다음 K개의 줄에는 사과의 위치가 주어지는데, 첫 번째 정수는 행, 두 번째 정수는 열 위치를 의미한다. 사과의 위치는 모두 다르며, 맨 위 맨 좌측 (1행 1열) 에는 사과가 없다.

다음 줄에는 뱀의 방향 변환 횟수 L 이 주어진다. (1 ≤ L ≤ 100)

다음 L개의 줄에는 뱀의 방향 변환 정보가 주어지는데,  정수 X와 문자 C로 이루어져 있으며. 게임 시작 시간으로부터 X초가 끝난 뒤에 왼쪽(C가 'L') 또는 오른쪽(C가 'D')로 90도 방향을 회전시킨다는 뜻이다. X는 10,000 이하의 양의 정수이며, 방향 전환 정보는 X가 증가하는 순으로 주어진다.

<br>

### 출력

---

첫째 줄에 게임이 몇 초에 끝나는지 출력한다.

<br>

### 예제 입력과 출력

---

```java
//입력
6
3
3 4
2 5
5 3
3
3 D
15 L
17 D
```

```java
//출력
9
```

<br>

### 풀이

---

- NxN크기의 2차원 배열에서 사과가 있는 곳은 2로 표시하고 뱀의 몸이 있는 곳은 1로 표시했다. (뱀이 움직이다가 벽 또는 자신의 몸에 부딪힐 경우 프로그램을 종료하기 때문에, 뱀의 몸이 있는 위치를 기록해야만한다.)

- 처음 프로그램을 시작할 때는 뱀이 오른쪽을 향한다. 뱀의 반환정보 (X,C)를 받아서 X초 후에는 뱀의 방향을 C(D or L)로 90도 회전시킨다.

  ```java
  //방향관련 배열
  //0:위쪽방향  1:아랫쪽 방향  2:오른쪽방향  3.왼쪽방향
  //direction[i][0] => 현재방향이 i일때 왼쪽으로 돌았을 때의 방향.
  //direction[i][1] => 현배방향이 i일때 오른쪽으로 돌았을때의 방향
  int[][] direction = {{3,2},{2,3},{0,1},{1,0}};
  
  //방향이 i일 때 (0:위쪽방향  1:아랫쪽 방향  2:오른쪽방향  3.왼쪽방향) 
  //다음 스텝을 가는 방법 => x+step[i][0], y+step[i][1]
  int[][] step = {{-1,0,},{1,0},{0,1},{0,-1}};	    
  ```

- 뱀의 머리가 이동하다가 사과가 없는 칸에 도착했을 때는 몸길이를 줄여서 꼬리가 위치한 칸을 비워줘야 한다. 따라서 꼬리의 위치를 기억하고 있어야 한다. 처음 뱀의 길이가 1일 때부터 그 위치를 큐에 기록한다. 이동한 칸에 사과가 존재하면 꼬리는 그대로 두고 길이를 늘리기 때문에, 현재 위치를 큐에 추가해 나간다. 이렇게 하면 큐에 맨 처음 부분에는 제일 먼저 들어온 꼬리의 위치 정보가 존재한다. 나중에 사과가 없는 칸을 만나면 큐에서 꼬리를 꺼내서 없앤다(FIFO).    

<br>

전체코드

```java
public class Main {

	public static void main(String[] args) throws NumberFormatException, IOException {
		// TODO Auto-generated method stub

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		int[][] map = new int[n][n];
		int k = Integer.parseInt(br.readLine());
		//사과는 2, 몸이 있는 곳은 1
		for(int i=0; i<k; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			map[Integer.parseInt(st.nextToken())-1][Integer.parseInt(st.nextToken())-1] = 2;
		}
	    int l = Integer.parseInt(br.readLine());
	    //X는 10,000 이하의 양의 정수
	    String[] rotateInfo = new String[10001]; //i초 후에 방향전환 정보 => rotateInfo[i]
	    for(int i=0; i<l; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			rotateInfo[Integer.parseInt(st.nextToken())] = st.nextToken();
	    }
	   
	    
	    //방향관련 배열
	    //0:위쪽방향  1:아랫쪽 방향  2:오른쪽방향  3.왼쪽방향
	    //direction[i][0] => 현재방향이 i일때 왼쪽으로 돌았을 때의 방향.
	    //direction[i][1] => 현배방향이 i일때 오른쪽으로 돌았을때의 방향
	    int[][] direction = {{3,2},{2,3},{0,1},{1,0}};
	    
	    //dir이 i일 때 다음 스텝을 가는 방법 => x+step[i][0], y+step[i][1]
		  int[][] step = {{-1,0,},{1,0},{0,1},{0,-1}};
	    
	    
	    //init
	    Queue<Pair> q =new LinkedList<>();  
	    int x = 0; int y = 0; 
	    map[x][y] = 1;
		  q.add(new Pair(x,y));
	    int dir = 2; 
	    
	    
		int time = 0;
		while(true) {
			time++;
			int curx = x+step[dir][0];
			int cury = y+step[dir][1];
			//만약 이동한 위치가 map을 벗어나거나, 자신의 몸과 부딪히면 게임이 끝난다.
		    if(curx<0 || curx>=n || cury<0 || cury>=n || map[curx][cury]==1) break;
		    
			if(map[curx][cury]!=2) { 
				Pair p = q.remove(); 
				map[p.x][p.y] = 0;
			}
			
			q.add(new Pair(curx,cury));
			map[curx][cury] = 1;
			x = curx; y = cury;
			
			//방향전환정보가 존재하는 경우
			if(time<=10000 && rotateInfo[time]!=null) {
				if(rotateInfo[time].equals("D")) {  //오른쪽 회전
		        	dir = direction[dir][1];
			    }else{  //왼쪽 회전
			        dir = direction[dir][0];
			    }
			}
		
		
		}
		
	    System.out.println(time);		
	}
		
}

class Pair{
	int x;
	int y;
	Pair(int x, int y){
		this.x = x;
		this.y = y;
	}
}

```

