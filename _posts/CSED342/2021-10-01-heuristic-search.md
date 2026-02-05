---
layout: post
title: "[CSED342] Review Heuristic Search"
date: 20211001
published: false
---

### To solve Search Problems

search problem을 해결하기 위해서는 다음 조건들이 clear하게 정의되어야 한다.

- state space: space에서 어느 위치에 존재하는지
- actions, costs
- successor function: action이나 cost에 대한 함수
- start state, goal test

### State Space Graphs

- node: 현재 world의 상태를 보여줌
- arc: action에 대한 결과 == successor
- goal test: goal node의 set

state space graph에서는 각 state가 단 한번씩만 일어난다. 전체 그래프를 그리기에는 너무 크기 때문에 그래프를 다 그려서 문제를 해결하기 보다는, 그래프라는 컨셉을 활용하여 적용해야 함!

### Search Trees

one-by-one으로 tree의 node가 생성된다. each node를 search할 때마다 children으로부터 가지가 뻗어나오는? -> state space graph와 다르게 node가 중복하여 등장할 수 있음.

- start state: root node
- successors: children
- node: state를 보여주는데 PLAN은 아님(?)

## Tree Search

### DFS(Depth-First Search)

- time complexity: O(b<sup>m</sup>)
- space complexity: O(bm)
- is complete: YES / m이 infinite할 수 있지만 cycle을 막으면 가능
- is optimal: NO / depth나 cost를 고려하지 않기 때문

### BFS(Breadth-First Search)

- time complexity: O(b<sup>s</sup>)
- space complexity: O(b<sup>s</sup>)
- is complete: YES / s가 finite
- is optimal: YES / cost가 모두 1일때만!

### UCS(Uniform Cost Search)

cost를 고려했을 때 BFS와 DFS가 섞인 serch 방법. 해당 cost를 만족하는 depth에 대해(DFS) BFS를 도는 방식이다. lowest path cost로 확장하는 방식.

- time complexity: O(b<sup>C*/ε</sup>)
- space complexity: O(b<sup>C*/ε</sup>)
- is complete: YES
- is optimal: YES

근데! 이제 문제는 uniform cost를 바로 못찾아가는 경우가 있음. 그래서 search에 대한 information이 존재하는 search 방식이 나옴.

## Informed Search

### Heuristics

얼마나 goal과 가까운지 estimate하는 함수 (manhattan distance, euclidean distance)

근데 계산 잘 해야지 아니면 딴길로 샐 수 있음

### Greedy Search

DFS와 유사하지만 이제 heuristic으로 예측한 ndoe로 뻗어나가는

### A*

UCS와 Greedy를 결합한 방식

- UCS: backward cost g(n) / 유사 BFS
- Greedy: forward cost h(n) / 유사 DFS
- A*: f(n) = g(n) + h(n)

A*는 언제 terminate 하냐?: goal을 dequeue하면서 최적의 goal을 찾아야 함

A*가 Optimal 하려면?: heuristic < actual

A*의 Optimality 계산하는 ppt 이해하기!

### Admissible Heuristics

heuristic이 admissible하려면 다음 조건을 만족해야 한다.

0 <= h(n) <= h<sup>*</sup>(n)

이떄 h<sup>*</sup>(n)는 가까운 goal까지 가는 실제 cost를 의미한다. 근데 그러면 goal에 도달하기 전까지는 확실히 알 수 없음.

### Creating Admissible Heuristics

search problem을 optimal하게 해결함에 있어 어려운 점은 admissible heuristic 값을 찾아내는 작업이다.

보통은 relaxed problem(단순화해서 constraint를 없애는)의 solution이 admissible heuristic이다.

현실의 문제를 해결함에 있어서는 Inadmissible heuristics가 사용되는 경우도 있음.

### A* 알고리즘의 trade-off

quality <-> computing

좋은 heuristic 값을 설정해주면(quality) 실제 cost와 근접할 수 있지만, heuristic을 계산함에 있어 computing이 많이 필요하다.

### Consistency of Heuristics

graph search를 함에 있어 admissibility뿐만 아니라 consistency도 고려해 주어야 optimal한 값을 구할 수 있다.

<hr>

## A* Summary

f(n) = g(n) + h(n)

admissible / consistent heuristic일 경우 optimal

heuristic design: 보통은 relaxed problem