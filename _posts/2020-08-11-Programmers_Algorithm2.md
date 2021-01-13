---
layout: post
title: (2020 KAKAO BLIND RECRUITMENT) 자물쇠와 열쇠
tags:
  - 알고리즘(Programmers)
---

<br>

### [문제](https://programmers.co.kr/learn/courses/30/lessons/60059)

---

고고학자인 **튜브**는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 **자물쇠**로 잠겨 있었고 문 앞에는 특이한 형태의 **열쇠**와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가 **`1 x 1`**인 **`N x N`** 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 **`M x M`** 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

<br>

### 제한사항

---

- key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
- lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
- M은 항상 N 이하입니다.
- key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
  - 0은 홈 부분, 1은 돌기 부분을 나타냅니다.
- key를 시계 방향으로 90도 회전하고, 오른쪽으로 한 칸, 아래로 한 칸 이동하면 lock의 홈 부분을 정확히 모두 채울 수 있습니다.

<br>

### 예제 입력과 출력

---

| key                               | lock                              | result |
| --------------------------------- | --------------------------------- | ------ |
| [[0, 0, 0], [1, 0, 0], [0, 1, 1]] | [[1, 1, 1], [1, 1, 0], [1, 0, 1]] | true   |

<br>

### 풀이

---

- 완전탐색
- 자물쇠는 1(돌기)과 0(홈)으로 이루어져 있다. 자물쇠의 모든 홈(0)을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있고, 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안된다. 그렇기 때문에 자물쇠와 열쇠의 값을 더했을 때 자물쇠의 모든 요소가 1로 이루어져 있으면 자물쇠를 열수 있다는 뜻이다. 
- 열쇠는 시계방향을 90되 회전이 가능하기 때문에 회전함수를 하나 만들어서 4번 회전 시켰다. (90회전,180회전,270회전,360회전(원본))
- 열쇠를 자물쇠에 넣어보는 과정을 편하게 하기 위해 자물쇠의 크기를 3배로 늘렸다. 그리고 새로운 자물쇠의 정중앙을 기존 자물쇠 정보로 채움
- 회전한 각각의 key마다 3배 커진 자물쇠를 이동하면서 정중앙에 있는 자물쇠가 열리는지 체크해준다. 

```java
class Solution {
    int n;
    int m;
    int[][] lock2;
    public boolean solution(int[][] key, int[][] lock) {
        boolean answer = false;
        n = lock.length;
        m = key.length;
        lock2 = new int[n*3][n*3];
        
        //자물쇠의 크기를 기존의 3배로 한 lock2배열을 만든다.
        //새로운 자물쇠의 중앙을 기존 자물쇠로 초기화
        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){
                lock2[n+i][n+j] = lock[i][j];
            }
        }
      
        
        //key를 4번 회전시킴. 각각의 key마다 자물쇠를 열수있는지 확인
        //하나라도 열수 있으면 return true;
        for(int k=1; k<=4; k++){
            key = rotate(key);
            for(int i=0; i<=n*3-m; i++){
                for(int j=0; j<=n*3-m; j++){
                     if(check(i,j,key)) return true;
                }
            }
        }
        
        return answer;
    }
  
  
    public boolean check(int x, int y, int[][] key){
         
        //자물쇠를 열쇠에 끼워보기 
         for(int i=0; i<m; i++){
             for(int j=0; j<m; j++){
                 lock2[x+i][y+j] += key[i][j];
             }
         }
         
        //가운데 lock확인해보기 -> 자물쇠가 모두 1로 될 경우만 true
        if(isAllOne()) return true;
        
        //조건을 만족하지 않는 key일 경우에는 lock을 원래대로 돌려놓고 false반환
        for(int i=0; i<m; i++){
             for(int j=0; j<m; j++){
                 lock2[x+i][y+j] -= key[i][j];
             }
         }
        return false; 
        
    }
   
    public boolean isAllOne(){
        for(int i=n; i<2*n; i++){
            for(int j=n; j<2*n; j++){
                if(lock2[i][j]!=1) return false;
            }
        }
        return true;
    }
    
    public int[][] rotate(int[][] key){
        int row = key.length;
        int col = key[0].length;
        int[][] ret = new int[col][row];
        
        for(int i=0; i<row; i++){
            for(int j=0; j<col; j++){
                ret[j][row-i-1] = key[i][j];
            }
        }
        return ret; 
    }
}
```

<br>

<br>

- 다시 풀었을 때

```java
class Solution {
    
    public int n,m;
    public int[][] map;
    public int count = 0;  //홈의 갯수
    
    public boolean solution(int[][] key, int[][] lock) {
       
        n = lock.length;
        m = key.length;
        map = new int[n*3][n*3];
            
         for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){
                 map[i+n][j+n] = lock[i][j];
                 if(lock[i][j]==0) count++; //홈갯수세기
            }
        }
        
        
        for(int r=0; r<4; r++){ //4번의 회전
            key = rotate(key);
            for(int i=0; i<=n*3-m; i++){
              for(int j=0; j<=n*3-m; j++){
                 if(check(i,j,key)) return true;
              }
            }
        }
        
        return false;
    
        
    }
    
  
    public boolean check(int x, int y, int[][] key){  
        int temp = 0;
        for(int i=0; i<m; i++){
            for(int j=0; j<m; j++){
                if(key[i][j]==1 && map[i+x][j+y]==1) return false; 
                else if(key[i][j]==1 && map[i+x][j+y]==0 && n<=i+x && i+x<2*n && n<=j+y && j+y<2*n) temp++; 
            }
        }
        return temp==count;   //자물쇠 안 모든 홈이 채워졌음을 의미
    }
    
    
  
    public int[][] rotate(int[][] key){ //시계방향 90도 회전한 배열 리턴
        int[][] new_key = new int[m][m];
        for(int i=0; i<m; i++){
            for(int j=0; j<m; j++){
                new_key[j][m-i-1] = key[i][j];
            }
        }
        return new_key;
    }
  
  
}
```

