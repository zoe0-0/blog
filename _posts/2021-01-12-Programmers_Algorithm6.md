---
layout: post
title: (2020 KAKAO BLIND RECRUITMENT) 가사 검색 (version_1)
tags:
  - 알고리즘(Programmers)
---

<br>

### [문제](https://programmers.co.kr/learn/courses/30/lessons/60060)

---

친구들로부터 천재 프로그래머로 불리는 **프로도**는 음악을 하는 친구로부터 자신이 좋아하는 노래 가사에 사용된 단어들 중에 특정 키워드가 몇 개 포함되어 있는지 궁금하니 프로그램으로 개발해 달라는 제안을 받았습니다.
그 제안 사항 중, 키워드는 와일드카드 문자중 하나인 '?'가 포함된 패턴 형태의 문자열을 뜻합니다. 와일드카드 문자인 '?'는 글자 하나를 의미하며, 어떤 문자에도 매치된다고 가정합니다. 예를 들어 `"fro??"`는 `"frodo"`, `"front"`, `"frost"` 등에 매치되지만 `"frame"`, `"frozen"`에는 매치되지 않습니다.

가사에 사용된 모든 단어들이 담긴 배열 `words`와 찾고자 하는 키워드가 담긴 배열 `queries`가 주어질 때, 각 키워드 별로 매치된 단어가 몇 개인지 **순서대로** 배열에 담아 반환하도록 `solution` 함수를 완성해 주세요.

<br>

### 가사 단어 제한사항

---

- `words`의 길이(가사 단어의 개수)는 2 이상 100,000 이하입니다.
- 각 가사 단어의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
- 전체 가사 단어 길이의 합은 2 이상 1,000,000 이하입니다.
- 가사에 동일 단어가 여러 번 나올 경우 중복을 제거하고 `words`에는 하나로만 제공됩니다.
- 각 가사 단어는 오직 알파벳 소문자로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.

<br>

### 검색 키워드 제한사항

---

- `queries`의 길이(검색 키워드 개수)는 2 이상 100,000 이하입니다.
- 각 검색 키워드의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
- 전체 검색 키워드 길이의 합은 2 이상 1,000,000 이하입니다.
- 검색 키워드는 중복될 수도 있습니다.
- 각 검색 키워드는 오직 알파벳 소문자와 와일드카드 문자인 `'?'` 로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.
- 검색 키워드는 와일드카드 문자인 `'?'`가 하나 이상 포함돼 있으며, `'?'`는 각 검색 키워드의 접두사 아니면 접미사 중 하나로만 주어집니다.
  - 예를 들어 `"??odo"`, `"fro??"`, `"?????"`는 가능한 키워드입니다.
  - 반면에 `"frodo"`(`'?'`가 없음), `"fr?do"`(`'?'`가 중간에 있음), `"?ro??"`(`'?'`가 양쪽에 있음)는 불가능한 키워드입니다.

<br>

### 입출력 예

---

| words                                                     | queries                                         | result            |
| --------------------------------------------------------- | ----------------------------------------------- | ----------------- |
| `["frodo", "front", "frost", "frozen", "frame", "kakao"]` | `["fro??", "????o", "fr???", "fro???", "pro?"]` | `[3, 2, 4, 1, 0]` |

<br>

### 입출력 예에 대한 설명

---

- `"fro??"`는 `"frodo"`, `"front"`, `"frost"`에 매치되므로 3입니다.
- `"????o"`는 `"frodo"`, `"kakao"`에 매치되므로 2입니다.
- `"fr???"`는 `"frodo"`, `"front"`, `"frost"`, `"frame"`에 매치되므로 4입니다.
- `"fro???"`는 `"frozen"`에 매치되므로 1입니다.
- `"pro?"`는 매치되는 가사 단어가 없으므로 0 입니다.

<br>

### 풀이

---

>  효율성 테스트가 있기 때문에 시간복잡도를 생각해서 구현해야 하는 문제였다. 우선 나는 이 문제를 풀 때 `Trie`자료구조에 대한 지식이 없었기 때문에 문제를 푸는데 꽤 어려움이 있었다. 정확성만 생각해서 문제를 푼다면, 검색 키워드 배열인 `queries` 를 돌면서 매 검색 키워드마다 `words` 배열에 들어있는 가사 단어들이 자신의 검색 키워드와 매치되는 단어인지 모두 확인하면 될 것이다. 하지만 이렇게 접근할 경우, 검색 키워드와 가사 단어를 비교하는 연산을 제외하고도 약 100억 번( `queries의 길이(100,000) * words의 길이(100,000)`  의 연산이 수행된다. 그래서 나는 이진탐색으로 시간을 줄일 수 있는 방법을 생각해보았고, 지난번에 공부했던 [upper_bound 와 lower_bound](https://zoe0-0.github.io/blog/BinarySearch_Alogorithm/) 로 문제를 해결해 보았다. 아직 Trie를 활용해서 문제를 풀어보지 않았기 때문에 이 방법이 얼마나 효율적인지는 모르겠지만 일단 채점은 통과할 수 있었다. 하지만 코드가 많이 복잡하고 지저분한 느낌이 든다... 다음번에 Trie로 구현한 알고리즘을 포스팅 할 예정이다.

<br>

- case1. 모든 단어가 '?' 로 이루어진 경우. 예를 들어 `?????`
  - 이 때는 키워드 `?????` 와 길이가 같기만 하면 모두 매칭 성공 
  - 가사 단어와 키워드를 비교할 필요 없이 길이가 같은 가사 단어의 갯수를 구하면 된다.

- case2. 접두사가 알파벳, 접미사가 '?' 를 포함하는 경우. 예를들어 `fro??`
  case2. 접두사는 알파벳, 접미사는 '?' 를 포함하는 경우. 예를 들어 `fro??`

  - 우선 키워드('fro??')와 길이가 같은 가사 단어들의 정렬된 리스트를 준비한다. (문자열 오름차순 정렬. Collections.sort 사용)

  - 키워드  `fro??` 에서 물음표를 제외한 prefix  `fro`  를 추출한다.

  - 정렬된 가사 단어 리스트를 이진 탐색으로 접근하는데, 이때 가사 단어들을 prefix와 비교하기 위해 substring

    - prefix가  `fro` , 원본 가사 단어가  `frant` 였다면, `"frant".substring(0,prefix.length())`를 통해  가사 단어 중  `fra` 만 추출한다. 이제  `fro` 와  `fra` 를 비교하면 된다. 

  - 먼저 `lower_bound`의 경우, 해당 prefix로 시작하는 단어 중 가장 왼쪽에 있는 인덱스를 리턴하도록 했다

    - 만약 해당 prefix로 시작하는 단어가 없다면 문자열 오름차순 정렬상 prefix 바로 다음에 위치한 인덱스를 리턴한다

    - 문자열 비교를 위해서 `compareTo()` 메소드를 사용했다. 

    ```java
    //String word = list.get(mid).substring(0,prefix.length());
    //int compareNum = word.compareTo(prefix);
    //compareNum ==0 word와 prefix가 일치할 때 
    //compareNum <0  정렬 순서가 word,prefix 순으로
    //compareNum >0  정렬 순서가 prefix,word 순으로 
    ```

    ```java
    int lower_bound(List<String> list, String prefix){
      int start = 0;
      int end = list.size()-1;
    
      while(start<end){
        int mid = (start+end)/2;
        String word = list.get(mid).substring(0,prefix.length());
    
        int compareNum = word.compareTo(prefix);
    
        if(compareNum >= 0){  
          end = mid;   
        }else{ 
          start = mid+1; 
        }
    
      }//while
    
      return end;
    }
    ```

   -  `upper_bound`는 해당 prefix로 시작하는 단어보다 오름차순 정렬상 바로 다음에 위치한 단어의 인덱스를 리턴한다
     ```java
     int upper_bound(List<String> list, String prefix){
       int start = 0;
       int end = list.size()-1;
     
       while(start<end){
         int mid = (start+end)/2;
         String word = list.get(mid).substring(0,prefix.length());
     
         int compareNum = word.compareTo(prefix);
     
         if(compareNum <= 0){  //word==prefix, word<prefix
           start = mid+1;
         }else { //word>prefix
           end = mid;
         }
     
       }//while
     
       return end;
     }
     ```

  - 이제 `upper_bound -  lower_bound` 값을 구하면 prefix로 시작하는 단어의 갯수를 구할 수 있다.

- case3. 접두사는 '?' 를 포함하고, 접미사는 알파벳으로 이루어진 경우. 예를 들어 `????o`
  - 가사 단어와 키워드 단어를 뒤집은 후, 위 case2와 동일한 방법으로 비교
  - 별도로 가사 단어를 reverse 하지 않고 구현하고 싶었지만, 너무 복잡해서 그냥 이렇게 했다

<br>

### 전체 코드 

```java
import java.util.*;

class Solution {
    public int[] solution(String[] words, String[] queries) {
        
     
        Map<Integer,List<String>> map = new HashMap<>();  
        Map<Integer,List<String>> rmap = new HashMap<>(); //좌우 반전시킨 단어들을 저장하기 위함
        
        for(int i=0; i<words.length; i++){
            String word = words[i];
            int len = word.length();
            
            if(!map.containsKey(len)) map.put(len,new ArrayList<>());
            if(!rmap.containsKey(len)) rmap.put(len,new ArrayList<>());
            
            map.get(len).add(word); 
            rmap.get(len).add(new StringBuilder(word).reverse().toString());  //단어 거꾸로 넣기
          
        }
        
        //map의 리스트 정렬하기
        for(Integer key : map.keySet()){
            Collections.sort(map.get(key));
            Collections.sort(rmap.get(key));
        }
        
        
        int[] ans = new int[queries.length];
        for(int i=0; i<queries.length; i++){
            String qr = queries[i];
            int len = qr.length();
            
            if(!map.containsKey(len)){  //해당 쿼리 길이의 단어가 아예 없으면
                ans[i] = 0;
                continue;
            }
            
            int count = 0;
            if(qr.startsWith("?") && qr.endsWith("?")){
                //????? -> 해당 길이의 모든 단어의 갯수
                count = map.get(len).size();
            }else if(qr.endsWith("?")){
                //fro?? -> map.get(key)
                String prefix = qr.substring(0,qr.indexOf("?"));
                count = getWordCnt(map.get(len),prefix);
            }else{
                 //??odo -> rmap.get(key)
                String prefix = qr.substring(qr.lastIndexOf("?")+1,len);
                prefix = new StringBuilder(prefix).reverse().toString();
                count = getWordCnt(rmap.get(len),prefix);
            }
            
          ans[i] = count;
        }
        
        return ans;
    }
    
    
    //해당 접두사로 시작하는 단어의 갯수 구하기
    //list내에 있는 단어들 중 prefix로 시작하는 단어의 갯수 리턴해줌
    int getWordCnt (List<String> list, String prefix){
                
        int lb = lower_bound(list,prefix);
        int ub = upper_bound(list,prefix);
        
        if(list.get(ub).startsWith(prefix)) ub++;
        return (lb==ub)?0:ub-lb;
    }
    
    
    int lower_bound(List<String> list, String prefix){
        int start = 0;
        int end = list.size()-1;
        
        while(start<end){
            int mid = (start+end)/2;
            String word = list.get(mid).substring(0,prefix.length());
            
            int compareNum = word.compareTo(prefix);
            
            if(compareNum >= 0){ 
                end = mid;
            }else{
                start = mid+1;
            }
            
        }//while
        
        return end;
    }
    
    
    
    
    int upper_bound(List<String> list, String prefix){
        int start = 0;
        int end = list.size()-1;
        
        while(start<end){
            int mid = (start+end)/2;
            String word = list.get(mid).substring(0,prefix.length());
            
            int compareNum = word.compareTo(prefix);
            
            if(compareNum <= 0){  
                start = mid+1;
            }else { 
                end = mid;
            }
            
        }//while
        
        return end;
    }
    
    
}
```



