---
layout: post
title: (백준 알고리즘_1748) 수 이어쓰기 1
tags:
  - 알고리즘(BOJ)
---

<br>

문제

---

1부터 N까지의 수를 이어서 쓰면 다음과 같이 새로운 하나의 수를 얻을 수 있다.

1234567891011121314151617181920212223...

이렇게 만들어진 새로운 수는 몇 자리 수일까? 이 수의 자릿수를 구하는 프로그램을 작성하시오.

<br>

입력

---

첫째 줄에 N(1≤N≤100,000,000)이 주어진다.

<br>

풀이

---

N의 최댓값이 100,000,000이기 때문에 1부터 N까지 for문을 돌리는 방식은 x

1-9 : 1x9개, 10-99 : 2x90개, 100-999 : 3x900 ... 를 이용

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Boj1748 {

	public static void main(String[] args) throws NumberFormatException, IOException {
		// TODO Auto-generated method stub

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String num = br.readLine();
		int len = num.length();
		
		int sum = 0;
		int temp = 9;
		for(int i=1; i<len; i++) {
			sum += i*temp;
			temp *= 10;
		}
		
		sum += len*(Integer.parseInt(num)-Math.pow(10,len-1)+1);
		System.out.println(sum);
	}

}
```

