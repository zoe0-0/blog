---
layout: post
title: (2018 KAKAO BLIND RECRUITMENT) 추석 트래픽
tags:
  - 알고리즘(Programmers)
---

<br>

### [문제](https://programmers.co.kr/learn/courses/30/lessons/17676)

---

### 문제 설명

이번 추석에도 시스템 장애가 없는 명절을 보내고 싶은 어피치는 서버를 증설해야 할지 고민이다. 장애 대비용 서버 증설 여부를 결정하기 위해 작년 추석 기간인 9월 15일 로그 데이터를 분석한 후 초당 최대 처리량을 계산해보기로 했다. **초당 최대 처리량**은 요청의 응답 완료 여부에 관계없이 임의 시간부터 1초(=1,000밀리초)간 처리하는 요청의 최대 개수를 의미한다.

<br>

### 입력 형식

- `solution` 함수에 전달되는 `lines` 배열은 **N**(1 ≦ **N** ≦ 2,000)개의 로그 문자열로 되어 있으며, 각 로그 문자열마다 요청에 대한 응답완료시간 **S**와 처리시간 **T**가 공백으로 구분되어 있다.
- 응답완료시간 **S**는 작년 추석인 2016년 9월 15일만 포함하여 고정 길이 `2016-09-15 hh:mm:ss.sss` 형식으로 되어 있다.
- 처리시간 **T**는 `0.1s`, `0.312s`, `2s` 와 같이 최대 소수점 셋째 자리까지 기록하며 뒤에는 초 단위를 의미하는 `s`로 끝난다.
- 예를 들어, 로그 문자열 `2016-09-15 03:10:33.020 0.011s`은 "2016년 9월 15일 오전 3시 10분 **33.010초**"부터 "2016년 9월 15일 오전 3시 10분 **33.020초**"까지 "**0.011초**" 동안 처리된 요청을 의미한다. **(처리시간은 시작시간과 끝시간을 포함)**
- 서버에는 타임아웃이 3초로 적용되어 있기 때문에 처리시간은 **0.001 ≦ T ≦ 3.000**이다.
- `lines` 배열은 응답완료시간 **S**를 기준으로 오름차순 정렬되어 있다.

<br>

### 출력 형식

- `solution` 함수에서는 로그 데이터 `lines` 배열에 대해 **초당 최대 처리량**을 리턴한다.

<br>

### 입출력 예

---

- 입력: [
  "2016-09-15 01:00:04.002 2.0s",
  "2016-09-15 01:00:07.000 2s"
  ]
- 출력: 2
- 설명: 처리시간은 시작시간과 끝시간을 **포함**하므로
  첫 번째 로그는 `01:00:02.003 ~ 01:00:04.002`에서 2초 동안 처리되었으며,
  두 번째 로그는 `01:00:05.001 ~ 01:00:07.000`에서 2초 동안 처리된다.
  따라서, 첫 번째 로그가 끝나는 시점과 두 번째 로그가 시작하는 시점의 구간인 `01:00:04.002 ~ 01:00:05.001` 1초 동안 최대 2개가 된다.

<br>

### 풀이

---

- 문자열 파싱후 시간 단위를 밀리세컨드로 통일시키고, 작업 완료시간과 작업을 처리하는데 걸린 시간을 이용해서 작업 시작시간을 구한다.

  - hour -> ms : `*60*60*1000`
  - minute->ms : `60*1000`
  - second->ms : `*1000`

  <br>

- 매 작업마다 작업 완료시간을 기준으로 1초동안 `(작업완료시간<= ... <=작업완료시간 +999ms) `  몇 개의 요청을 처리했는지 카운트
- 이때 `(작업완료시간<= ... <=작업완료시간 +999ms) `이 구간에서 처리하는 요청은
  - 응답 시작 시간이 구간에 걸쳐 있거나
    - 기준작업의 응답완료시간 <=  작업의 시작시간  <=기준작업의 응답완료시간+999
  - 응답 완료 시간이 구간에 걸쳐 있거나
    - 기준작업의 응답완료시간 <=  작업의 완료시간  <=기준작업의 응답완료시간+999
  - 응답 처리 시간이 구간에 걸쳐 있는 경우
    - 작업의 시작시간<기준작업의 완료시간  &&  기준작업의 완료시간+999<작업의 완료시간 
- 가장 큰 카운트 값을 기록한다. 

<br>

### 전체 코드 

```java
class Solution {
    
    public int solution(String[] lines){
     
        int len = lines.length;
        int[] sm = new int[len];  //작업 시작시간을 기록 (ms단위)
        int[] em = new int[len];  //작업 완료시간을 기록 (ms단위)
        int ans = 0;
        
        //1.parsing
        for(int i=0; i<len; i++){
            String[] arr = lines[i].split("\\s"); //공백기준으로 나누기
    
            int end = 0;
            String[] time =  arr[1].split(":");
            end += Integer.parseInt(time[0])*60*60*1000;  //hour-> ms
            end += Integer.parseInt(time[1])*60*1000;  //minute -> ms
            end += Integer.parseInt(time[2].substring(0,2))*1000; //second -> ms
            end += Integer.parseInt(time[2].substring(3,6)); //ms

            int start = end+1;
            String workingTime = arr[2].substring(0,arr[2].length()-1); //맨뒤 s제거하기
            int index = workingTime.indexOf(".");
            if(index==-1) {
                start -= Integer.parseInt(workingTime)*1000;  //second -> ms
            }else{
                start -= Integer.parseInt(workingTime.substring(0,index))*1000; //second -> ms
                start -= Integer.parseInt(workingTime.substring(index+1,workingTime.length())); //ms
            }
            
            sm[i] = start;
            em[i] = end;
        }
        
      
      
        //2.초당 최대 처리량 구하기
        //lines배열은 응답완료시간을 기준으로 오름차순 정렬되어있다
        for(int i=0; i<len; i++){
            int curEndTime = em[i]; 
            int curStartTime = sm[i];
            int count = 0;
            
            for(int j=i; j<len; j++){ //현재작업의 응답완료시간 <= 다음작업의 응답완료시간
                int nextEndTime = em[j];
                int nextStartTime = sm[j];
              
                if(curEndTime<= nextStartTime && nextStartTime<=curEndTime+999) count++;
                else if(curEndTime<= nextEndTime && nextEndTime<=curEndTime+999) count++;
                else if(nextStartTime<curEndTime && curEndTime+999 <nextEndTime) count++;
                
                ans = Math.max(ans,count);
            }//for
          
        }//for
        
        return ans;
        
    }
    
    

}  
```



