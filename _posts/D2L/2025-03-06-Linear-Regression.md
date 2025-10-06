---
layout: post
title: "[D2L] Linear Regression"
date: 20250306
published: false
---

# Linear Regression
Regression은 하나 이상의 독립변수에 (independent variable)와 그에 대한 종속변수 (dependent variable) 사이의 관계를 모델링하는 방식이다. 기계학습에서 regression은 주로 숫자 (numerical value)를 예측하는 데에 사용된다.

## Basic Elements of Linear Regression
- training set, training dataset: 모델을 학습하는 데에 사용되는 데이터셋
- example, sample: 데이터셋의 각 행
- label, target: 모델을 통해 추정하고자 하는 값
- features, covariates: 추정하는 데에 사용되는 독립변수
- prediction, inference: 만든 모델을 이용하여 target을 추정하는 행위

$i$ 번째 index의 sample과 그에 해당하는 label은 다음과 같이 수식으로 나타낼 수 있다.
$$
\mathbf{x}^{(i)} = [x_{1}^{i}, x_{2}^{i}]^{T}, y^{(i)}
$$

## Linear Model
모델에 주어지는 값이 $d$개의 feature를 가지고 있다고 했을 때 우리가 추정하고자 하는 target은 다음과 같이 나타낼 수 있다. 여기서 $w$는 weight, $b$는 bias를 의미한다.
$$
\hat{y} = w_{1}x_{1} + ... + w_{d}x_{d} + b
$$

$$
\hat{y} = \mathbf{w}^{T}\mathbf{x} + b
$$

Linear regression의 목적은 위의 수식에서 추정의 에러를 최소화할 수 있는 weight와 bias를 찾는 것이다. 이를 model parameter라고 부르며, 최선의 model parameter를 구하기 위해서는 다음의 요소가 필요하다.

1. 모델의 quality를 측정할 수 있는 방법: Loss Function
2. 모델의 quality를 개선할 수 있도록 모델을 업데이트할 수 있는 방법: ??

## Loss Function
Loss function을 통해 우리는 모델의 fitness를 계산할 수 있다. Loss function은 실제 값 (real value)과 추정 값 (predicted value) 사이의 거리를 계산한다. 그렇기 때문에 보통 loss는 (1) 음이 아닌 정수이고, (2) 0에 가까울수록 모델의 성능이 좋음 (fitness가 좋음)을 시사한다.

가장 유명하고 자주 사용되는 loss function은 squared error이다. 하나의 example $i$에 대한 loss function은 다음과 같다.

$$
l^{i}(\mathbf{w}, b) = \frac{1}{2} \left(\hat{y}^{(i)} - y^{(i)}\right)^{2}
$$

Linear regression의 목적은 loss 값을 최소화하는 것이기 때문에 주로 주어진 loss function을 미분한다. 위의 식에 있는 계수 $\frac{1}{2}$는 미분했을 때 계산을 더 편하게 만들어 준다.

$n$ 개의 전체 데이터에 대한 loss function은 다음과 같이 각각에 대한 loss를 구한 뒤 average를 구하면 된다.

$$
L(\mathbf{w}, b) = \frac{1}{n} \sum_{i=1}^{n} l^{(i)} (\mathbf{w}, b) = \sum_{i=1}^{n} \frac{1}{2} \left(\mathbf{w}^{T}\mathbf{x}^{(i)} + b - y^{(i)}\right)^{2}
$$

모델을 학습함에 있어 우리의 목적은 total loss ($L(w, b)$)를 최소화할 수 있는 parameter $(\mathbf{w}^{*}, b^{*})$를 찾는 것이 목적이기 때문에 $argmin$을 사용하여 다음과 같이 나타낼 수 있다.

$$
\mathbf{w}^{*}, b^{*} = \underset{\mathbf{w}, b}{\arg\min} L(\mathbf{w}, b)
$$

## Analytic Solution
Linear regression은 간단한 optimization 문제로, analytic solution을 통해 해결할 수 있다.

## Minibatch Stochastic Gradient Descent
어떤 모델을 analytic하게 해결할 수 없을 때는 gradient descent를 통해 loss function을 줄일 수 있도록 parameter를 업데이트하면 된다. 이 때 모든 example에 대해 gradient descent를 계산하게 되면 너무 많은 비용이 들기 때문에 random sampling을 통해 sample minibatch ($\Beta$)를 설정하고 gradient descent를 적용하는 minibatch stochastic gradient descent 방식을 사용한다.

아래 식에서 $|\Beta|$는 minibatch의 size를, $\eta$는 learning rate를 의미한다. Batch의 size나 learning rate와 같이 모델을 학습하는 동안 변화하지 않는 변수를 hyperparameter라고 부른다.

$$
(\mathbf{w}, b) \leftarrow (\mathbf{w}, b) - \frac{\eta}{|\Beta|}\sum_{i=\Beta} \partial_{(\mathbf{w},b)} l^{(i)} (\mathbf{w}, b)
$$


$$
\mathbf{w} \leftarrow \mathbf{w} - \frac{\eta}{|\Beta|}\sum_{i=\Beta} \mathbf{x}^{(i)} \left(\mathbf{w}^{T}\mathbf{x}^{(i)} + b - y^{(i)} \right)
$$

$$
b \leftarrow b - \frac{\eta}{|\Beta|}\sum_{i=\Beta} \mathbf{x}^{(i)} \left(\mathbf{w}^{T}\mathbf{x}^{(i)} + b - y^{(i)} \right)
$$

모델을 학습시킬 때 유한한 step을 통해서는 minimum에 도달할 수 없다. 또, training data를 기준으로 학습을 하기 때문에 test data에 대해서도 잘 작동할 수 있는지를 고려해야 한다. 이렇게 한 번도 본 적 없는 데이터에 대해서도 잘 작동하는 모델을 만드는 것을 generalization이라고 부른다.
