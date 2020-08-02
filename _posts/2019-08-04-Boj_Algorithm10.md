---
layout: post
title: (백준 알고리즘_1541) 잃어버린 괄호
tags:
  - 알고리즘(BOJ)
---

<br>

### 문제

---

세준이는 양수와 +, -, 그리고 괄호를 가지고 길이가 최대 50인 식을 만들었다. 그리고 나서 세준이는 괄호를 모두 지웠다.

그리고 나서 세준이는 괄호를 적절히 쳐서 이 식의 값을 최소로 만들려고 한다.

괄호를 적절히 쳐서 이 식의 값을 최소로 만드는 프로그램을 작성하시오.

<br>

### 입력

---

첫째 줄에 식이 주어진다. 식은 ‘0’~‘9’, ‘+’, 그리고 ‘-’만으로 이루어져 있고, 가장 처음과 마지막 문자는 숫자이다. 그리고 연속해서 두 개 이상의 연산자가 나타나지 않고, 5자리보다 많이 연속되는 숫자는 없다. 수는 0으로 시작할 수 있다.<br>

### 출력

---

첫째 줄에 정답을 출력한다.

<br>

### 예제 입력과 출력

---

```java
#입력
55-50+40
```

```java
#출력
-35
```

<br>

### 풀이

---

가장 작은 값을 출력하기 위해서는 -부호를 만났을 때 다음번 -부호가 나오기 전까지 중간에 있는 수들을 다 더해서 빼주면 된다

예시) 50-30+10+20-10 => 50-(30+10+20)-(10) 

이렇게 하면 뺄 수 있는 수의 크기가 커지기 때문에 결과값이 가장 작게 나온다. 

<br>

1. 첫번째 풀이

```java
public class Boj1541 {

	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String str = br.readLine();
		char sign = '+';
		StringBuilder sb = new StringBuilder();
		int sum = 0;
		
		for(int i=0; i<str.length(); i++) {
		   char ch = str.charAt(i);
		   if(ch!='+' && ch!='-') {
			   sb.append(ch);
			   continue;
		   }
		   
		   int num = Integer.parseInt(sb.toString());
		   sb.delete(0,sb.length());
		   
		   if(sign=='+') {
			   sum += num;
			   sign = ch;
		   }else {
			   sum -= num;
			   sign = ch;
			   if(sign=='+') sign = '-';
		   }
		   
		}
		
		if(sign=='+') sum += Integer.parseInt(sb.toString());
		else sum -= Integer.parseInt(sb.toString());
		
		System.out.println(sum);
		
		
	}

}

```

<br>

2. 두번째 풀이

```java
public class Boj1541_v2 {

	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] input = br.readLine().split("\\-");
		
		int ans = 0;
		for(int i=0; i<input.length; i++) {
		    int sum = getSum(input[i]);
		    if(i==0) ans += sum;
		    else ans -= sum;
		}
		
		System.out.println(ans);
		
		
	}
	
	static int getSum(String str) {
		int ret = 0;
		String[] nums = str.split("\\+");
		for(int i=0; i<nums.length; i++) {
			ret += Integer.parseInt(nums[i]);
		}
		return ret;
	}

}
```

- 주의할 점 : `String[] nums = str.split("\\+");`이 부분에서 `Dangling meta character '+' near index 0` 에러발생
- split("") 안에 특수문자를 그대로 넣었을 때 발생하는 오류. 해결방법은 `String[] nums = str.split("\\+");`