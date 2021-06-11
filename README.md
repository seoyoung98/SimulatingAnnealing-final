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

#### 1. 3~4차 함수의 전역 최적점을 찾을 수 있는 모의 담금질 기법 구현
##### Main
```
public class Main {
    public static void main(String[] args) {
        SimulatedAnnealing sa = new SimulatedAnnealing(10);
        Problem p = new Problem() {
            @Override
            public double fit(double x) {
                return x*x*x -3*x*x - 2*x + 2;
            }

            @Override
            public boolean isNeighborBetter(double f0, double f1) {
                return f0 < f1;
            }
        };
        double x = sa.solve(p, 100, 0.99, 0, 0, 10);
        System.out.println(x);
        System.out.println(p.fit(x));
        System.out.println(sa.hist);
    }
}
```
##### Problem
```
public interface Problem {
    double fit(double x);
    boolean isNeighborBetter(double f0, double f1);
}
```

##### Simulating Annealing
```
import java.util.ArrayList;
import java.util.Random;

public class SimulatedAnnealing {
    private int niter;
    public ArrayList<Double> hist;

    public SimulatedAnnealing(int niter) {
        this.niter = niter;
        hist = new ArrayList<>();
    }

    public double solve(Problem p, double t, double a, double lower, double upper) {
        Random r = new Random();
        double x0 = r.nextDouble() * (upper - lower) + lower;
        return solve(p, t, a, x0, lower, upper);
    }

    public double solve(Problem p, double t, double a, double x0, double lower, double upper) {
        Random r = new Random();
        double f0 = p.fit(x0);
        hist.add(f0);

        for (int i=0; i<niter; i++) {
            int kt = (int) t;
            for(int j=0; j<kt; j++) {
                double x1 = r.nextDouble() * (upper - lower) + lower;
                double f1 = p.fit(x1);

                if(p.isNeighborBetter(f0, f1)) {
                    x0 = x1;
                    f0 = f1;
                    hist.add(f0);
                } else {
                    double d = Math.sqrt(Math.abs(f1 - f0));
                    double p0 = Math.exp(-d/t);
                    if(r.nextDouble() < 0.0001) {
                        x0 = x1;
                        f0 = f1;
                        hist.add(f0);
                    }
                }
            }
            t *= a;
        }
        return x0;
    }
}
```

#### 결과
 > 전역 최적값 : 9.98435431397806 <br>
 > 전역 최적값의 결과 값 : 3664.2238341240172<br>
![image](https://user-images.githubusercontent.com/80522538/121679630-827c9b00-caf3-11eb-8818-567d3c535bf2.png)

#### 2. curve fitting을 위한 선형 또는 비선형 모델
##### 모델 선정
 > 독립변수 : 공부한 시간<br>
 > 종속변수 : 시험 결과<br>
 >            F-0 , D-1.0, C0-2.0, C+-2.5, B0-3.0,B+-3.5, A0-4.0, A+-4.5

##### Main함수
```
public class Main {
    public static void main(String[] args) {
        SimulatedAnnealing sa = new SimulatedAnnealing(10);
        Problem p = new Problem() {
            @Override
            public double fit(double x) {
                return 0.001*x*x + 0.25*x;
            }

            @Override
            public boolean isNeighborBetter(double f0, double f1) {
                return f0 < f1;
            }
        };
        double x = sa.solve(p, 100, 0.99, 0, 0, 16);
        System.out.println(x);
        System.out.println(p.fit(x));
        System.out.println(sa.hist);
    }
}
```

##### 결과
전역 최적값 : 15.998392936786242 <br>
전역 최적값의 결과값 : 4.255546810756372 <br>
![image](https://user-images.githubusercontent.com/80522538/121683708-b8704e00-caf8-11eb-80e3-8b90f8294d1d.png)

 - 공부를 많이 할수록 학점이 높아지게 된다.






