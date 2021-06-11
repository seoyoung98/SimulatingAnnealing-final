# SimulatingAnnealing
## 모의 담금질 기법
#### 모의 담금질 기법
 - 해를 탐색하기 위해서, 후보해에 대해 이웃하는 해를 정의
 - 높은 온도에서는 나쁜 해로 이동하는 자유로움을 보임
 - 낮은 온도로 갈수록 나쁜 해로 이동하는 확률이 줄어듦 <br>
 - 지역 최적해에서 더 이상 아래로 탐색할 수 없는 상태에 이르렀을 때 위 방향으로 이동하다가 전역 최적해를 찾은 것<br>
![image](https://user-images.githubusercontent.com/80522538/121670888-e3eb3c80-cae8-11eb-99fa-ffbe084ad3f5.png)


### 문제 
 > 3~4차 함수의 전역 최적점을 찾을 수 있는 모의 담금질 기법을 구현하고
 > 하나의 독립변수로 설명되는 종속변수 데이터를 구한 후
 > curve fitting을 위한 선형 또는 비선형 모델을 선정한다

#### 모의 담금질 기법 구현
