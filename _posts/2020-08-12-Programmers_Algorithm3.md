---
layout: post
title: (2019 KAKAO BLIND RECRUITMENT) 실패율
tags:
  - 알고리즘(Programmers)
---

<br>

### [문제](https://programmers.co.kr/learn/courses/30/lessons/42889)

---

슈퍼 게임 개발자 오렐리는 큰 고민에 빠졌다. 그녀가 만든 프랜즈 오천성이 대성공을 거뒀지만, 요즘 신규 사용자의 수가 급감한 것이다. 원인은 신규 사용자와 기존 사용자 사이에 스테이지 차이가 너무 큰 것이 문제였다.

이 문제를 어떻게 할까 고민 한 그녀는 동적으로 게임 시간을 늘려서 난이도를 조절하기로 했다. 역시 슈퍼 개발자라 대부분의 로직은 쉽게 구현했지만, 실패율을 구하는 부분에서 위기에 빠지고 말았다. 오렐리를 위해 실패율을 구하는 코드를 완성하라.

- 실패율은 다음과 같이 정의한다.
  - 스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수

전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록 solution 함수를 완성하라.

<br>

### 제한사항

---

- 스테이지의 개수 N은 `1` 이상 `500` 이하의 자연수이다.
- stages의 길이는 `1` 이상 `200,000` 이하이다.
- stages에는 `1`이상 `N + 1` 이하의 자연수가 담겨있다.
  - 각 자연수는 사용자가 현재 도전 중인 스테이지의 번호를 나타낸다.
  - 단, `N + 1` 은 마지막 스테이지(N 번째 스테이지) 까지 클리어 한 사용자를 나타낸다.
- 만약 실패율이 같은 스테이지가 있다면 작은 번호의 스테이지가 먼저 오도록 하면 된다.
- 스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 `0` 으로 정의한다.

<br>

### 예제 입력과 출력

---

| N    | stages                   | result      |
| ---- | ------------------------ | ----------- |
| 5    | [2, 1, 2, 6, 2, 4, 3, 3] | [3,4,2,1,5] |
| 4    | [4,4,4,4,4]              | [4,1,2,3]   |

<br>

### 풀이

---

```java
import java.util.*;
class Solution {
    public int[] solution(int N, int[] stages) {
        int[] stageUserCnt = new int[N+2];
        for(int i=0; i<stages.length; i++){
            stageUserCnt[stages[i]]++;
        }
      
        StageInfo[] info = new StageInfo[N];
        int userCnt = stages.length;
        
        for(int i=1; i<=N; i++){
            if(userCnt>0){
               info[i-1] = new StageInfo(i,(float)stageUserCnt[i]/userCnt);
               userCnt-=stageUserCnt[i]; 
            }else{
                info[i-1] = new StageInfo(i,0);
            }
        }
        
        Arrays.sort(info);
  
        int[] answer = new int[N];
        for(int i=0; i<N; i++){
            answer[i] = info[i].stage;
        }
        
        return answer;
        
    }
}


//실패율 내림차순. 실패율이 같을 경우 스테이지 번호 오름차순
class StageInfo implements Comparable<StageInfo>{
    int stage; //스테이지
    float fail;  //실패율
    StageInfo(int stage,float fail){
        this.stage = stage;
        this.fail = fail;
    }
  
  @Override
	public int compareTo(StageInfo other) {
				if(this.fail!=other.fail){
            return Float.compare(other.fail, this.fail);
        }else{
            return Integer.compare(this.stage, other.stage);
        }
	}
  
}
```



