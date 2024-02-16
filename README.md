# FM
# 0. Abstract

- Factorization Machines은 SVM의 장점들을 가진 새로운 행렬 분해 기반 모델이다.
- SVM과 같이 일반적인 인풋을 받을 수 있다.
    - 일반적인 인풋: SVD++의 경우 유저, 아이템, 평점, 그리고 이 아이템에 리뷰를 달았는지의 유무를 입력받는다. FM의 경우 이러한 부가적인 정보를 도메인에 대한 특별한 지식 없이 쉽게 추가할 수 있다.
- SVM과는 다르게 모든 변수는 행렬 인수분해를 사용한다.
- 그렇기 때문에 희소성이 큰 데이터에서도 사용이 가능하다.
- FM 모델은 선형적인 시간복잡도를 갖음

# 1. Introduction

앞으로 말할 내용

1. SVM이 왜 매우 희소한 데이터 환경, 높은 차원에서 실패하는지
2. 기존의 행렬 분해 모델들의 한계: 일반적이지 못한 인풋
3. SVMs와는 다르게 dense parametrization을 사용하지 않고 factorized parametrization을 사용
4. 어떻게 선형적인 시간복잡도로 계산이 가능한지, 선형적인 개수의 인자로 가능한지.
5. SVMs들은 최적화하기 위해 dual form을 사용

장점 정리

1. FMs는 SVMs가 실패하는 매우 희소한 데이터 환경에서도 동작한다.
2. FMs는 선형적인 시간 복잡도를 갖는다.
3. general predictor이다

# 2. Prediction Under Sparsity

- 랭킹 스코어하는 함수는 pairwise training data를 통해 얻을 수 있다.
- (Xa, Xb)에서 Xa가 Xb보다 높은 랭킹을 갖고 있다고 표현한다.
- 랭킹 관계는 반대칭관계이다.(서로 교환할 수 없음)
- 랭킹 관계는 positive training instances만 있어도 충분하다.

# 3. Factorization Machines

## A. Factorization Machine Model

### 1) Model Equation:

$
\hat{y}(X) := {w}_{0} + \sum_{i=1}^nw_{i}x_{i} + \sum_{i=1}^{n}\sum_{j=i+1}^{n}\langle v_i, v_j \rangle x_i x_j
$

$
\langle v_i, v_j \rangle := \sum _{f=1}^{k} v_{i,f} \cdot v_{j,f}
$

여기서 k는 하이퍼파라미터이다.

- $w_0$은 글로벌 바이어스로 상수(전체 평점의 평균)
- $w_i$는 i번째 변수에 대한 가중치
- $\langle v_i, v_j \rangle$은 i번째 변수와 j번째 변수의 상호 작용을 나타낸다.

### 2) Expressiveness(표현력)

- 잘 알려져있듯 positive definite matrix W는 V를 갖는다.
- $W = V \cdot V^t$ (여기서 k는 충분히 커야함)
- 하지만 데이터가 희소한 환경에서는 W를 표현하기 위한 충분한 데이터가 있지 않기 때문에 k값을 제한해야한다.

### 3) Parameter Estimation Under Sparsity

- 보틍은 데이터가 sparse한 환경에서 변수간의 관계를 직접적, 독립적으로 표현할 수 없다.
- 하지만 FMs는 변수간의 독립성을 내적으로 없애기 때문에 데이터간의 관계를 포착할 수 있다.
    - MF에서 두 변수 간의 관계를 low rank로 분해하는 과정과 비슷한듯

### 4) Computation

- 일련의 과정을 통해 $O(kn^2)$ → O(kn)으로 변환