---
layout: post
title: "[CSED342] Review CSP-game-MDP"
date: 20211006
published: false
---

## Constraint Satisfaction Problems

- search problem의 일종
- state는 domain(D)에 속하는 variable로 정의됨
- goal test는 constraint를 만족해야 함

### CSP with search methods

- BFS: 무조건 leaf node까지 방문해야 알 수 있음(CSP는 optimal을 찾는 것이 아니라 constraint를 만족하는 경우만 찾으면 됨)
- DFS: constraint를 만족시키지 못하면 dead end가 발생할 수 있음 -> backtracking을 할 수 이쏘록 해야 함!

=> search를 하되, constraint check을 효율적으로 할 수 있도록 구성해야 한다!!

### Backtracking Search

1. One variable at at time: variable assignment는 순서 변경이 가능하다!
2. Check constraints as you go: 이전 assignment와 conflict를 일으키지 않는 value를 설정해 주어야 한다!

> dead end를 좀 더 빨리 알아차리면 backtracking에서 소모하는 cost를 감소시킬 수 있지 않을까?


<!-- 20p부터 하기! 우선 minimax부터 복습해야겠음 ㅇㅇ -->

## Adversarial Search

### Minimax

- DFS를 이용함
- player들은 turn을 바꿔가며 행동함
- each node의 minimax value를 계산하여 움직임

minimax는 다음 세 함수를 이용하여 구현할 수 있다.

```python
def max-value(state):
    initialize v = -{infinite}
    for each successor of state:
        v = max(v, mini-value(successor))
    return v
```

```python
def min-value(state):
    initialize v = +{infinite}
    for each successor of state:
        v = min(v, max-value(successor))
    return v
```

```python
def value(state):
    if the state is a terminal state: return the state utility
    if the next agent is MAX: return max-value(state)
    if the next agent is MIN: return min-value(state) 
```

minimax를 이용하였을 때 DFS만큼의 time, space complexity를 가지기 때문에 방문하지 않아도 될 node는 미리 제거하는 방식을 사용하면 좋음!

### Alpha-Beta Pruning