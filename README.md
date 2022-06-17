# Simulated Annealing

### 회귀식 추정
 회귀식 추정은 한 개 또는 그 이상의 변수들 X(독립변수)에 대하여 다른 한 변수Y(종속변수) 사이의 관계를 설명하고 예측하는 분석기법 입니다.<br>
산점도의 점들의 분포를 통해 일정한 패턴을 확인한 후, 상관계수를 구하여 두 변수 간의 선형관계를 알 수 있습니다.<br>
이 일정한 패턴을 활용하여 무엇인가를 예측하는 분석을 회귀식 추정이라고 합니다.<br>
simulated annealing(모의 담금질 기법) 알고리즘으로 에러를 최소화하는 모수값을 추정해보겠습니다.<br>

### 모의 담금질 기법이란
 앞서 제가 모의 담금질 기법에 대하여 알아보겠다 했으니 담금질 기법에 대해 간략하게 설명해보겠습니다.<br> 담금질 기법은 광대한 탐색 공간 안에서 해를 반복하여 개선해나감으로써, 주어진 함수의 에러를 최소화하는 모수값 즉, 전역 최적해를 찾는 것 입니다. 여기서 해를 반복하여 개선해 나간다고 하였는데 모의 담금질 기법에서는 이 방법에서 교수님이 강의 때 말씀해주신 온도를 씁니다. 온도가 클 때는 분자 운동이 활발하여 애들이 크게 움직이지만 점점 온도가 식어감으로써 운동량이 줄어들어 변화가 줄어듭니다.<br> 이 온도를 이용하여 지역 최적해 빠질 수 있는 것을 방지하여 전역 최적해를 찾는 것 입니다.

* * *
``` 

import random
import math

def fi(x):
    return x*x-3*x+2  

t = 90
a=0.95
tf = 0.001

while t > tf:
    history = []
    x = random.random()*100  # 후보 해를 구하기 위해 독립변수 x를 random함수를 이용하여 만든다.
    f = fi(x) # 후보 해를 만들기 위해 값을 넣는다.
    history.append(f)     # 후보해들을 추적하기 위해 리스트에 넣어둔다.
    p = int(2/t)       # p는 온도가 내려갈수록 증가하게 만들었다. 온도가 내려가면 크게 움직일 가능성이 낮기 때문에 반복횟수를 더 주는 것이다. 
    for j in range(p):   
        rand = random.random()
        x2 = rand*100   # 이웃 해를 구하기 위해 독립변수 x2를 random함수를 이용하여 만든다.
        f2 = fi(x2)  # 이웃 해를 구한다.
        if f>f2 :     # 비교해서 후보 해가 더 높으면 
            x = x2   # 이웃 독릭변수를 후보 독릭변수에다가 넣어준다.
            f = f2   # 후보 해의 값을 이웃 해로 교체
            history.append(f)     # 그리고 바뀐 후보해를 추적하기 위해 리스트에 넣어준다.
        else:       # 이 부분이 있는 이유는 지역 최적해 빠질 수 있는 것을 방지하기 위해 있다.
            percentage = math.exp(-(f2 - f)/t)     #percentage를 구해준다.
            if rand<percentage:       # 만약 percentage가 rand값보다 크다면 후보해를 바꿔줄 수 있다.
                x = x2     
                f = f2    
                history.append(f)     # 후보해들을 추적하기 위해 리스트에 넣어준다.
    t *= a      # 반복할 때마다 온도를 낮춰준다.

print("전역 최적해 : " , f)
print("경로 추적 : " ,history)

```

* 코드 설명 <br>
후보해를 임시로 설정해주기 위하여 random라이브러리와 exp를 쓰기위해 math 라이브러리를 임포트 해줍니다.<br> 초창기 온도를 90도로 높게 설정해주고 온도를 반복할 때마다 낮추기 위해 0.95로 해줍니다. 온도가 계속 낮아지면 반복횟수가 많아지면서 최적해를 정밀하게 찾을 수 있지만 너무 낮추다보면 반복횟수가 늘어나 시간 복잡도의 효율성이 떨어지기 때문에 온도의 마지노선은 0.001로 설정해줍니다. <br> 현재의 온도가 온도의 마지노선 값보다 클 경우 반복을 돕니다. history 리스트는 후보 해들을 추적하기 위하여 만든 리스트입니다. random함수를 이용하여 처음 독립변수를 만들어주고 온도에 반응하여 반복할 변수 p를 만들어줍니다. 이웃해를 구할 x2도 랜덤함수를 이용하여 구해준다. 그리고 바로 이웃해를 구해줍니다. 후보해가 새로 구한 이웃해보다 더 높다면 x2를 x로 f2를 f로 바꿔줍니다. 그러는 이유는 최저점을 찾기 위해 이렇게 하는 것입니다. 최고점을 구하고 싶다면 저 부호만 반대로 해주시면 됩니다. 근데 그럴라면 함수도 위로 오목한 함수로 바꿔야합니다.<br> else 구문이 없다면 지역 최적해 빠질 수가 있다. 계속 구하는 이웃해가 후보해보다 높다면 이동하지 못하고 그냥 지역 최적점에 빠져있기 때문입니다. 여기서 온도에 근거하여 확률을 구해줍니다.<br> random함수를 이용하여 만들어놓은 rand 확률보다 만약 온도에 근거하여 만든 확률이 더 높다면 움직일 수 있게 해주는 겁니다. 그리고 반복 마지막에서는 현재 온도에다가 a를 곱하영 온도를 조금씩 식혀줍니다. 마지막에 저 무한루프를 빠져나왔을 때의 마지막 f즉 후보값이 전역 최적점입니다. <br>

* * *
* 출력값
![이차함수 출력값](https://user-images.githubusercontent.com/87864025/174228725-6a771a47-b21a-433d-be36-5915a73ea6b5.PNG)

<br>
그럼 담금질 기법을 이용하여 구한 지역 최적해가 에러를 최소한 모수값인지 함수 그래프를 보면서 비교해보겠습니다.
<br>

* 함수 그래프

![x^2-3x+2그래프그림](https://user-images.githubusercontent.com/87864025/174228955-c6b5f99d-9a14-440d-a58e-0122d8bfa017.png)

<br>
이 2차 함수에서의 최적점은 0.25이다. 출력값에서 구한 최적해와 2차 함수의 최적해가 근사한 걸로 보아 잘 돌아간 것입니다.<br>
하지만 이렇게 이차 함수로 구하면 잘 구해지지만 교수님이 강의 때 설명해주신 지역 최적해에 빠지지 않아 왜 온도라는 변수를 사용해야 하는지 설명이 되지 않습니다. 지역 최적해에 빠질라면 지역 최적해가 최소 2개 이상인 함수를 살펴봐야합니다. <br>
교수님이 강의 때 예로 4차 함수와 산을 올라 어느 봉우리가 제일 높은 봉우리인지 그림으로 설명해주셨습니다. 저는 4차함수로 설명해보겠습니다.

* 4차함수 그래프

![4차함수 그림](https://user-images.githubusercontent.com/87864025/174231647-7418a3bd-94b0-4693-be70-571a99738804.png)

<br>이 4차함수에서는 -3.048가 전역 최적해이고 0.947이 지역 최적해입니다. 

* 4차함수의 출력값

![4차함수 결과값](https://user-images.githubusercontent.com/87864025/174232143-92edce1d-1f8a-4274-962d-e489cad32c34.PNG)

<br> -3.046으로 지역 최적해에 빠지지 않고 전역 최적해를 잘 찾은 것을 알 수 있습니다.

* * *

### 선형 회귀식 추정
파이썬에서는 회귀식 분석을 위하여 여러가지 라이브러리들을 제공합니다. 그 중에서도 머신러닝에서 주로 쓰는 언어이기 때문에 데이터들까지 제공해줍니다.<br>
코드를 보여드리면서 설명드리겠습니다.

```
import pandas as pd
import numpy as np
from sklearn.datasets import load_diabetes
import matplotlib.pyplot as plt

data = load_diabetes()

df = pd.DataFrame(data['data'],index=data['target'],columns = data['feature_names'])

from sklearn.linear_model import LinearRegression
lr = LinearRegression()
y = df.index.values
x = df.bmi.values

x = x.reshape(-1,1)
y = y.reshape(-1,1)

lr.fit(x,y)

print("회귀식의 기울기는 : " ,lr.coef_[0])
print("회귀식의 y절편은 : " ,lr.intercept_)

plt.scatter(x, y)
plt.plot(x, y, color='red')
plt.title('y = {}*x + {}'.format(lr.coef_[0], lr.intercept_))
plt.show()
```
<br>
일단 맨 처음에 import한 라이브러리들은 따로 파이썬 터미널에서 설치해주셔야 사용하실 수 있습니다. 일단 먼저 pandas 라이브러리는 파이썬 언어로 작성된 데이터를 분석 및 조작하는 것입니다.<br> numpy 라이브러리는 다차원 배열을 쉽게 처리하고 효율적으로 사용할 수 있도록 해줍니다. 여기서 위에서 제가 데이터들을 제공해준다고 했는데 sklearn.datasets에서 제공해주는 것입니다.<br> 저는 그 중에서 442명의 당뇨병 환자들의 데이터를 가지고 왔습니다. 그 밑에 matplotlib라이브러리로 선형회귀를 그려줄 것입니다.<br>
data에다가 당뇨병 환자들의 데이터를 넣어줄 것입니다. 데이터는 사전형으로 구성되어있으며 여러가지 key들이 있다. 그 중에서 data, target, feature_names를 쓸 것이므로 df에다가 pd형태로 넣어준다. 여러가지 방법으로 회귀분석을 해 볼 수 있는데 제가 사용한 방법은 sklearn를 선형회귀입니다. sklearn에서 제공하는 linear_model을 import 합니다.ㅣ
lr = LinearRegression() lr 변수를 생성합니다. 그러면 lr로 간단하게 회귀분석을 할 수 있습니다.<br> 아까 위에서 정의한 데이터프레임 df에서 Y는 당뇨 수치가가 될 것이고, X는 bmi로 정할 것입니다. x와 y는 사전형으로 되어있기 때문에 2차원 배열로 바꿔주기 위해 reshape를 해줍니다. lr.fit을 함으로써 회귀식을 구할 수 있습니다. 그 다음 회귀식의 기울기와 y절편을 출력해줍니다. 그리고 위에서 선형회귀를 그리기 위해 import한 plt를 이용하여 그림을 그려주면 된다.
<br>

* 출력된 그림

![선형회귀 분석 글림](https://user-images.githubusercontent.com/87864025/174262741-5f35f427-d6e1-4737-a763-5289a178483e.PNG)


<br>

* 기울기와 y절편 출력값

![회귀식 기울기,y절편](https://user-images.githubusercontent.com/87864025/174263083-11bf25cf-b822-4c26-958b-57b09864dc68.PNG)

#### 실제 그래프와 비교
기울기와 y절편이 잘 구해진 것인지 실제 함수를 그려 비교해보겠습니다.
<br>
* 회귀식 함수

![션형회귀 함수그림](https://user-images.githubusercontent.com/87864025/174264062-db5fd205-c8ef-48e0-b520-3b14b403b2e5.png)

그래프를 보면 0.15일때 300에 살짝 아래인 것을 보아 294가 나왔으니 회귀식을 알맞게 추정한 것 같습니다.

한 학기동안 수고하셨습니다!(_ _)





