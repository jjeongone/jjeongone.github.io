---
layout: post
title: "[D2L] Generalization"
date: 20250318
published: false
---

# Generalization
Generalization은 모델이 새로운 데이터에서도 잘 작동하기 위해 갖추어야 하는 중요한 성질이다. 모델을 학습하는 데에 사용한 training set에서만 잘 작동하는 모델은 현실의 데이터를 마주했을 때에는 성능이 좋지 못할 것이기 때문에 좋은 모델이라고 할 수 없다. 머신러닝은 사실 데이터에 general하게 적용할 수 있는 pattern을 찾는 것이 목적이라고 할 수 있다.

Training set에서만 모델이 잘 작동하는 현상을 overfitting이라고 부르고, overfitting을 방지하기 위한 방법을 regularization이라고 부른다.

## Training Error and Generalization Error
기본적인 supervised learning 상황에서는 학습할 때 사용하는 training 데이터와 테스트할 때 사용하는 test data가 서로 identical한 distribution에서 independent하게 주어진다고 가정한다 (IID assumption). 