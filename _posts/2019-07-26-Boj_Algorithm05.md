---
layout: post
title: (백준 알고리즘_2138) 전구와 스위치
tags:
  - 알고리즘(BOJ)
---

<br>

문제

---

N개의 스위치와 N개의 전구가 있다. 각각의 전구는 켜져 있는(1) 상태와 꺼져 있는 (0) 상태 중 하나의 상태를 가진다. i(1<i<N)번 스위치를 누르면 i-1, i, i+1의 세 개의 전구의 상태가 바뀐다. 즉, 꺼져 있는 전구는 켜지고, 켜져 있는 전구는 꺼지게 된다. 1번 스위치를 눌렀을 경우에는 1, 2번 전구의 상태가 바뀌고, N번 스위치를 눌렀을 경우에는 N-1, N번 전구의 상태가 바뀐다.

N개의 전구들의 현재 상태와 우리가 만들고자 하는 상태가 주어졌을 때, 그 상태를 만들기 위해 스위치를 최소 몇 번 누르면 되는지 알아내는 프로그램을 작성하시오.

<br>

입력

---

첫째 줄에 자연수 N(2≤N≤100,000)이 주어진다. 다음 줄에는 전구들의 현재 상태를 나타내는 숫자 N개가 공백 없이 주어진다. 그 다음 줄에는 우리가 만들고자 하는 전구들의 상태를 나타내는 숫자 N개가 공백 없이 주어진다.

<br>

출력

---

첫째 줄에 답을 출력한다. 불가능한 경우에는 -1을 출력한다.

<br>

풀이

---

첫번째 전구는 첫번째 스위치와 두번째 스위치로 상태를 결정할 수 있다. 첫번째 전구의 스위치를 조작한(그대로 두거나, 버튼을 누르거나)후에 첫번째 전구를 변경할 수 있는 방법은 두번째 전구의 스위치를 조작하는 방법뿐이다. 

따라서 출발은 첫번째 전구를 그대로 두거나, 스위치를 누르는 방법(꺼져 있는 전구는 켜지고, 켜져 있는 전구는 꺼지게) 두가지로 나누어진다. 각각의 경우에서 최종적으로 첫번째 전구의 상태를 결정하는 키는 두번째 스위치가 가지고 있게 된다. 

만들고자 하는 첫번째 전구의 상태와 현재 첫번째 전구의 상태를 비교해서 두번째 스위치를 누를지 말지 결정한다. 만약 현재 첫번째 전구가 켜져 있는(1) 상태인데, 만들고자 하는 첫번째 전구의 상태는 꺼져 있는(0) 상태라면, 두번째 스위치를 눌러야한다.

두번째 전구는 첫번째 스위치와 두번째 스위치, 세번째 스위치로 상태를 결정할 수 있다. 그런데 앞에서 첫번째 스위치와 두번째 스위치의 조작방법을 이미 결정했다. 따라서 두번째 전구의 상태를 결정하는 키는 세번째 스위치가 가지고 있게 된다. 만들고자 하는 두번째 전구의 상태와 현재 두번째 전구의 상태를 비교해서 세번째 스위치를 누를지 말지 결정한다. 

...이런식으로 반복하면서 스위치를 조작해나간다.

단, 마지막 전구는 바로 앞 전구의 상태에 따라 누를지 말지를 결정하기 때문에, 만들고자 하는 마지막 전구와 상태가 같지 않을 수도 있다. 이런 경우는 조작이 불가능한 경우이다. 

<br>

```java
public class Boj2138 {

	static int ans = 100001;
  
	public static void main(String[] args) throws NumberFormatException, IOException {
    
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		int[] s1 = new int[n];
		int[] s2 = new int[n];		
		int[] end = new int[n];
    
		String line = br.readLine();
		for(int i=0; i<n; i++) {
			s1[i] = s2[i] = line.charAt(i)-'0';
		}
		line = br.readLine();
		for(int i=0; i<n; i++) {
			end[i] = line.charAt(i)-'0';
		}
	    
    //첫번째 스위치를 누른 경우
		s1[0] = 1 - s1[0];
		s1[1] = 1 - s1[1];
		go(s1,end,1);
    
    //그대로
		go(s2,end,0);

		if(ans==100001) ans = -1;
		System.out.println(ans);
	}
	
	static void go(int[] start, int[] end ,int count) {
		
		int len = start.length;
		
		for(int i=1; i<len; i++) { 
			if(start[i-1]==end[i-1]) continue;
			start[i-1] = 1 - start[i-1];
			start[i] = 1 - start[i];
			if(i+1<len) start[i+1] = 1 - start[i+1];
			++count;
		}
		
    //마지막 상태가 같은지 체크한다
		if(start[len-1] == end[len-1])  ans = Math.min(ans, count);
	}


}
```
