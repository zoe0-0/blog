---
layout: post
title: (백준 알고리즘_1339) 단어수학
tags:
  - 알고리즘(BOJ)
---

<br>

문제

---

민식이는 수학학원에서 단어 수학 문제를 푸는 숙제를 받았다.

단어 수학 문제는 N개의 단어로 이루어져 있으며, 각 단어는 알파벳 대문자로만 이루어져 있다. 이때, 각 알파벳 대문자를 0부터 9까지의 숫자 중 하나로 바꿔서 N개의 수를 합하는 문제이다. 같은 알파벳은 같은 숫자로 바꿔야 하며, 두 개 이상의 알파벳이 같은 숫자로 바뀌어지면 안 된다.

예를 들어, GCF + ACDEB를 계산한다고 할 때, A = 9, B = 4, C = 8, D = 6, E = 5, F = 3, G = 7로 결정한다면, 두 수의 합은 99437이 되어서 최대가 될 것이다.

N개의 단어가 주어졌을 때, 그 수의 합을 최대로 만드는 프로그램을 작성하시오.

<br>

입력

---

첫째 줄에 단어의 개수 N(1 ≤ N ≤ 10)이 주어진다. 둘째 줄부터 N개의 줄에 단어가 한 줄에 하나씩 주어진다. 단어는 알파벳 대문자로만 이루어져있다. 모든 단어에 포함되어 있는 알파벳은 최대 10개이고, 수의 최대 길이는 8이다. 서로 다른 문자는 서로 다른 숫자를 나타낸다.

<br>

출력

---

첫째 줄에 주어진 단어의 합의 최댓값을 출력한다.

<br>

풀이

---

* 각각의 알파벳에 매칭된 숫자를 출력할 필요가 없으므로, 순열을 구하지 말고 최댓값이 나올 수 있는 경우를 생각해보자

```7
해결방법
GCF + ACDEB의 최댓값을 구할 경우

  GCF = 100G + 10C + 1F
+ ACDEB = 10000A + 1000C + 100D + 10E + 1B
------------------------------------------
10000A + 1010C + 100G + 100D + 10E + 1F + 1B

가장 큰 자릿수 값을 가지는 알파벳 A부터 가장 큰 숫자 9를 매칭
A : 9 / C : 8 / G : 7 / D : 6 / E : 5 / F: 4 / B : 3

따라서, GCF + ACDEB의 최댓값은 99437
```

<br>

```java
import java.io.*;
import java.util.*;


public class Boj1339 {

	public static void main(String[] args) throws NumberFormatException, IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
		int[] arr = new int['Z'+1];
		
		for(int i=0; i<n; i++) {
			String str = br.readLine();
			for(int j=0; j<str.length(); j++) {
				char ch = str.charAt(j);
				int digit = (int) Math.pow(10,(str.length() -j -1));
				arr[ch] += digit;
			}
		}
		
		Arrays.sort(arr);
		int temp = 9;
		int sum = 0;
		for(int i=arr.length-1; i>=0; i--) {
		    if(arr[i]==0) break;
		    sum += arr[i]*temp--;
		}
		
		System.out.println(sum);
	}

}

```

