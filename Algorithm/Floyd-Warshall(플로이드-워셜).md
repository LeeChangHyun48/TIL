# 플로이드-워셜 알고리즘

###  * 음수 순환 사이클이 없는 그래프에서 모든 점에서 모든 점까지의 최단거리를 구하는 알고리즘
###  * 음의 가중치를 가져도 되나 합이 음수 가중치를 갖는 사이클이 있으면 안됨
###  * 3중 반복문을 사용하게 때문에 시간복잡도는 O(N^3)으로 느리다

## 구현
#### 가중치가 없는경우 그래프를 그리기 위해 자기 자신은 0으로 나머지는 INF 값으로 지정해준다.

### 코드
    INF = 9999999;
    N = 5;
    
    for(int i = 1; i <= N; i++){
      for(int j = 1; j <= N; j++){
        arr[i][j] = INF;

        if(i == j) {
          arr[i][j] = 0;
        }
      }
    }

1. INF를 Integer.MAX_VALUE로 하지 않는 이유는 오버플로우가 발생하여 값이 음수로 변동될 수 있기 때문
2. ex) ( 1,1 ) ( 2,2 ) ( 3,3 )... 같은 자기 자신은 0으로 지정하고 나머지는 INF로 만들어 준다.
3. 구현된 2차원 배열은 아래와 같은 이미지로 볼 수 있다.
![image](https://github.com/LeeChangHyun48/TIL/assets/116571873/ab8c2f05-c502-4d75-b6e9-63738c746bb5)



### 좌표 값을 넣어준다.

    x  y
    1  3
    1  4
    4  5
    4  3
    3  2

    for(int i = 0; i < 5; i++){
      arr[x][y] = 1;
      arr[y][x] = 1;
    }

1. 무방향 그래프일 경우 (x, y)와 (y, x)에 1의 가중치를 부여(가중치가 있을경우 따로 지정하여 부여)한다.
2. 새롭게 구현된 2차원 배열은 아래와 같은 이미지로 볼 수 있다.
![image](https://github.com/LeeChangHyun48/TIL/assets/116571873/3c47bf76-7849-49ea-ae02-ee8e953f3308)




### 플로이드 워셜 알고리즘 적용

    for(int k = 1; k <= N; k++){
      for(int i = 1; i <= N; i++){
        for(int j = 1; j <= N; j++){
          arr[i][j] = Math.min(arr[i][j], arr[i][k] + arr[k][j];
        }
      }
    }

1. k는 거쳐가는 경유지, i는 출발지, j는 도착지이다.
2. arr[i][j] = Math.min(arr[i][j], arr[i][k] + arr[k][j]를 사용하여 최단거리를 갱신해준다.
3. 아래 이미지 순으로 적용된다.
   
![image](https://github.com/LeeChangHyun48/TIL/assets/116571873/fc7a5fd9-7689-442f-89d0-c37f8563ea99) 
![image](https://github.com/LeeChangHyun48/TIL/assets/116571873/6104e483-601a-4965-8096-9327f573b8cc)

#### 경유 정점 1-2까지 최단거리 변화가없어 3부터 시작
#### 경유 정점( k ) 3번, 시작 정점( i ) 1번, 도착 정점 ( j ) 1 ~ 5번 일때
- arr[1][1] : arr[1][3] + arr[3][1] = 1 + 1 = 2      -> 0 : 2 으로 갱신 X
- arr[1][2] : arr[1][3] + arr[3][2] = 1 + 1 = 2      -> INF : 2 으로 갱신 O
- arr[1][3] : arr[1][3] + arr[3][3] = 1 + 0 = 1      -> 1 : 1 으로 갱신 X
- arr[1][4] : arr[1][3] + arr[3][4] = 1 + 1 = 2      -> 1 : 2 으로 갱신 X
- arr[1][5] : arr[1][3] + arr[3][5] = 1 + INF = INF  -> INF: INF 으로 갱신 X
  
![image](https://github.com/LeeChangHyun48/TIL/assets/116571873/b1ff2c27-a43f-47a0-8368-552feb4b1caf)

#### 경유 정점 ( k ) 3번, 시작 정점 ( i ) 2번, 도착 정점 ( j ) 1 ~ 5번 일때
- arr[2][1] : arr[2][3] + arr[3][1] = 1 + 1 = 2      -> INF : 2 으로 갱신 O
- arr[2][2] : arr[2][3] + arr[3][2] = 1 + 1 = 2      -> 0 : 2 으로 갱신 X
- arr[2][3] : arr[2][3] + arr[3][3] = 1 + 0 = 1      -> 1 : 1 으로 갱신 X
- arr[2][4] : arr[2][3] + arr[3][4] = 1 + 1 = 2      -> INF : 2 으로 갱신 O
- arr[2][5] : arr[2][3] + arr[3][5] = 1 + INF = INF  -> INF : INF 으로 갱신 X

![image](https://github.com/LeeChangHyun48/TIL/assets/116571873/04f37e98-cd67-40a6-9c4a-116fbcb546bc)

#### 경유 정점 ( k ) 3번, 시작 정점 ( i ) 3번, 도착 정점 ( j ) 1 ~ 5번 일때
- arr[3][1] : arr[3][3] + arr[3][1] = 0 + 1 = 1      -> 1 : 1 으로 갱신 X
- arr[3][2] : arr[3][3] + arr[3][2] = 0 + 1 = 1      -> 1 : 1 으로 갱신 X
- arr[3][3] : arr[3][3] + arr[3][3] = 0 + 0 = 0      -> 0 : 0 으로 갱신 X
- arr[3][4] : arr[3][3] + arr[3][4] = 0 + 1 = 1      -> 1 : 1 으로 갱신 X
- arr[3][5] : arr[3][3] + arr[3][5] = 0 + INF = INF  -> INF : INF 으로 갱신 X

#### 경유 정점 ( k ) 3번, 시작 정점 ( i ) 4번, 도착 정점 ( j ) 1 ~ 5번 일때
- arr[4][1] : arr[4][3] + arr[3][1] = 1 + 1 = 2      -> 1 : 2 으로 갱신 X
- arr[4][2] : arr[4][3] + arr[3][2] = 1 + 1 = 2      -> INF : 2 으로 갱신 O
- arr[4][3] : arr[4][3] + arr[3][3] = 1 + 0 = 1      -> 1 : 1 으로 갱신 X
- arr[4][4] : arr[4][3] + arr[3][4] = 1 + 1 = 2      -> 0 : 2 으로 갱신 X
- arr[4][5] : arr[4][3] + arr[3][5] = 1 + INF = INF  -> 1 : INF 으로 갱신 X

![image](https://github.com/LeeChangHyun48/TIL/assets/116571873/0aa6153a-8cc4-4f10-9a62-f8a2a783cb35)

#### 경유 정점 ( k ) 3번, 시작 정점 ( i ) 5번, 도착 정점 ( j ) 1 ~ 5번 일때
- arr[5][1] : arr[5][3] + arr[3][1] = INF + 1 = INF    -> INF : INF 으로 갱신 X
- arr[5][2] : arr[5][3] + arr[3][2] = INF + 1 = INF    -> INF : INF 으로 갱신 X
- arr[5][3] : arr[5][3] + arr[3][3] = INF + 0 = INF    -> INF : INF 으로 갱신 X
- arr[5][4] : arr[5][3] + arr[3][4] = INF + 1 = INF    -> 1 : INF 으로 갱신 X
- arr[5][5] : arr[5][3] + arr[3][5] = INF + INF = INF  -> 0 : INF 으로 갱신 X

#### 이러한 방식으로 경유 정점 5번까지 끝낸다면 두 정점간의 최단 거리가 남게 되고 다음과 같이 변하게 된다.
![image](https://github.com/LeeChangHyun48/TIL/assets/116571873/835b5985-70a1-4794-bb1a-cd2d90744f77)
