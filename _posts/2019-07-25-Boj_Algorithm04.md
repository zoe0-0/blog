---
layout: post
title: (백준 알고리즘_1285) 동전 뒤집기
tags:
  - 알고리즘(BOJ)
---

<br>

문제

---

N2개의 동전이 N행 N열을 이루어 탁자 위에 놓여 있다. 그 중 일부는 앞면(H)이 위를 향하도록 놓여 있고, 나머지는 뒷면(T)이 위를 향하도록 놓여 있다. <그림 1>은 N이 3일 때의 예이다.

![img](https://www.acmicpc.net/upload/images/qFb2xhF6yGeur.png)

이들 N2개의 동전에 대하여 임의의 한 행 또는 한 열에 놓인 N개의 동전을 모두 뒤집는 작업을 수행할 수 있다. 예를 들어 <그림 1>의 상태에서 첫 번째 열에 놓인 동전을 모두 뒤집으면 <그림 2>와 같이 되고, <그림 2>의 상태에서 첫 번째 행에 놓인 동전을 모두 뒤집으면 <그림 3>과 같이 된다.

![img](https://www.acmicpc.net/upload/images/kZHVpNcZWaEy.png)

<그림 3>의 상태에서 뒷면이 위를 향하여 놓인 동전의 개수는 두 개이다. <그림 1>의 상태에서 이와 같이 한 행 또는 한 열에 놓인 N개의 동전을 모두 뒤집는 작업을 계속 수행할 때 뒷면이 위를 향하도록 놓인 동전의 개수를 2개보다 작게 만들 수는 없다.

N2개의 동전들의 초기 상태가 주어질 때, 한 행 또는 한 열에 놓인 N개의 동전을 모두 뒤집는 작업들을 수행하여 뒷면이 위를 향하는 동전 개수를 최소로 하려 한다. 이때의 최소 개수를 구하는 프로그램을 작성하시오.

<br>

입력

---

첫째 줄에 20이하의 자연수 N이 주어진다. 둘째 줄부터 N줄에 걸쳐 N개씩 동전들의 초기 상태가 주어진다. 각 줄에는 한 행에 놓인 N개의 동전의 상태가 왼쪽부터 차례대로 주어지는데, 앞면이 위를 향하도록 놓인 경우 H, 뒷면이 위를 향하도록 놓인 경우 T로 표시되며 이들 사이에 공백은 없다.

<br>

출력

---

첫째 줄에 한 행 또는 한 열에 놓인 N개의 동전을 모두 뒤집는 작업들을 수행하여 뒷면이 위를 향하여 놓일 수 있는 동전의 최소 개수를 출력한다.

<br>

풀이

---

한 동전은 앞면 또는 뒷면 2개의 면을 가지고 있다. 따라서 한 동전을 계속 뒤집어도 H->T->H->T->H->T...처럼 그대로이거나, 뒤집히거나 이 두 가지 경우의 수만 존재한다. 또한 N^2개의 동전에 대하여 임의의 한 행 또는 한 열에 놓인 N개의 동전을 모두 뒤집는 작업을 수행할 수 있다고 한다.

따라서 한 동전의 상태를 결정하는 방법은, 해당 동전이 속한 <b>행</b>을 조작하거나(뒤집거나 그대로두거나), 해당 동전이 속한 <b>열</b>을 조작(뒤집거나 그대로 두거나)하는 두 가지의 방법이 있다. 

먼저 모든 행에 대하여 뒤집을지 그대로 둘지 결정했다면, 열에 대해서는 동전 뒷면의 개수가 최소가 되도록 결정하면 된다. 

<br>

```java
import java.io.*;
import java.util.*;

public class Boj1285 {

	static int min = 400;
	
	public static void main(String[] args) throws NumberFormatException, IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		char[][] coins = new char[n][n];
		for(int i=0; i<n; i++) {
			String line = br.readLine();
		    for(int j=0; j<n; j++) {
		    	coins[i][j]= line.charAt(j);
		    }
		}
		
		go(coins,0,n);
		System.out.println(min);
		
	}
	
	static void go(char[][] arr, int row_index, int n) {
		
		if(row_index==n) { //모든 행의 결정이 끝남. -> 최소의 뒷면이 나오도록 열을 결정함
		   int min_count = 0;
		   for(int c=0; c<n; c++) {
			   int tcount = 0;
			   for(int r=0; r<n; r++) {
				   if(arr[r][c]=='T') ++tcount;
			   }
			   if(tcount>n-tcount) {  //뒷면의 갯수가 앞면의 갯수보다 많을 경우에만 해당 열을 뒤집는다
				   reverseColumn(arr,c,n);
				   min_count += (n-tcount);
			   }else {
				   min_count += tcount;
			   } 
		   }	
		   
		   min = Math.min(min,min_count);
		 	return;
		}
		
		//현재 행을 뒤집지 않고 다음행으로 가거나
		go(arr,row_index+1,n);
		
		//현재 행을 뒤집고 다음행으로 가거나
		for(int i=0; i<n; i++) {
			if(arr[row_index][i]=='H') arr[row_index][i] = 'T';
			else arr[row_index][i] = 'H';
		} 
    go(arr,row_index+1,n);
		
	}
	
	static void reverseColumn(char[][] arr, int col_index, int n) {
		for(int r=0; r<n; r++) {
           if( arr[r][col_index] =='H' ) arr[r][col_index] = 'T';
           else arr[r][col_index] = 'H';
		}
	}
  
}
```
