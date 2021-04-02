---
layout: post
title: (알고리즘) 투 포인터(Two Pointers) 문제풀이
tags:
  - 알고리즘
---

<br>

### [백준2003번_수들의 합](https://www.acmicpc.net/problem/2003)

---

```java
public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int s = Integer.parseInt(st.nextToken());
        int[] arr = new int[n];
        st = new StringTokenizer(br.readLine());
        for(int i=0; i<n; i++) arr[i] = Integer.parseInt(st.nextToken());

        int end = 0;
        int sum = 0;
        int count = 0;

        for(int start=0; start<n; start++){
            while(sum<s && end<n) sum += arr[end++];

            if(sum==s) count++;
            sum -= arr[start];
        }

        System.out.println(count);

    }

}

```

<br>

### [백준1806번_부분합](https://www.acmicpc.net/problem/1806)

---

```java
public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int s = Integer.parseInt(st.nextToken());
        int[] arr = new int[n];
        st = new StringTokenizer(br.readLine());
        for(int i=0; i<n; i++) arr[i] = Integer.parseInt(st.nextToken());

        int end = 0;
        int sum = 0;
        int minLen = n+1;

        //합이 S 이상이 되는 것 중, 가장 짧은 것의 길이를 구하
        for(int start=0; start<n; start++){
            while(sum<s && end<n) sum += arr[end++];

            if(sum>=s){ //위 while문에서 sum<s이지만, end==n인 경우를 거르기 위해서
                minLen = Math.min(minLen,end-start);
                sum -= arr[start];
            }
        }

        int ans = (minLen==n+1) ? 0 : minLen;
        System.out.println(ans);

    }

}

```

<br>