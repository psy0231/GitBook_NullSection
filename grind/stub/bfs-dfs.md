# BFS, DFS

## Breadth First Search (BFS)

* 너비 우선 탐색(Breadth-first search, BFS).
* 맹목적 탐색방법의 하나
  * 맹목적 탐색(blind search)\
    이미 정해진 순서에 따라\
    상태 공간 그래프를 점차 형성해 가면서\
    해를 탐색하는 방법.
* 시작 노드를 방문한 후\
  시작 노드에 인접한 모든 노드들을\
  우선 방문하는 방법.
* 더 이상 방문하지 않은 노드가 없을 때까지\
  방문하지 않은 모든 노드들에 대해서도\
  너비 우선 검색을 적용.
* queue를 사용해 구현.

### 장단점

* 장점
  * 출발노드에서 목표노드까지의\
    최단 길이 경로를 보장.
* 단점
  * 경로가 매우 길 경우에는\
    탐색 가지가 급격히 증가함에 따라\
    보다 많은 기억 공간을 필요로 하게 된다.
  * 해가 존재하지 않는다면\
    유한 그래프(finite graph)의 경우에는\
    모든 그래프를 탐색한 후에 실패로 끝난다.
  * 무한 그래프(infinite graph)의 경우에는\
    결코 해를 찾지도 못하고, 끝내지도 못한다.

### python

```python
# parameter는 dict, int.
# graph는 Dict 자료형.  
# key는 노드, value는 인접노드.
def bfs(graph: dict, start: int):
    # visited : 방문 배열. 
    # visited[node] = True이면 
    # node는 방문이 끝난 상태.
    visited = {i: False for i in graph.keys()}
    queue = [start]
    visited[start] = True
    # visited_nodes : 방문한 노드를 리스트
    visited_nodes = [start]  

    # 큐가 빌 때까지 반복
    while len(queue) > 0:
        #queue에서 노드를 하나 빼 온다. 
        # 이 노드를 current라고 하자.
        current = queue.pop(0)
        print("Visited node:", current)
        # current의 인접 노드들을 확인한다. 
        # 이 각각의 노드를 nxt라고 하자.
        for nxt in graph[current]:
            # 만일 nxt에 방문하지 않았다면 
            # nxt 방문
            if not visited[nxt]: 
                queue.append(nxt)
                visited[nxt] = True
                visited_nodes.append(nxt)
    return visited_nodes
```

```python
def bfs(graph: dict, start: int):
    visited = {i: False for i in graph.keys()}
    queue = [start]
    visited[start] = True
    visited_nodes = [start]  

    while len(queue) > 0:
        current = queue.pop(0)
        print("Visited node:", current)
        for nxt in graph[current]:
            if not visited[nxt]: 
                queue.append(nxt)
                visited[nxt] = True
                visited_nodes.append(nxt)
    return visited_nodes
```

## Depth First Search (DFS)

* 깊이 우선 탐색(depth-first search, DFS)
* 탐색트리의 최근에 첨가된 노드를 선택하고,\
  이 노드에 적용 가능한 동작자 중 하나를 적용해\
  노드 다음 level에 자식노드 한 개를 추가하고\
  첨가된 자식 노드가 목표노드일 때까지\
  앞의 자식 노드의 첨가 과정을 반복.
* 일반적으로 재귀함수나 stack으로 구현.
* 위 설명이 뭔소린가 싶었는데\
  예를들어 미로에서 출구를 찾는다면.
  * '최근에 첨가된 노드'는 지금 서 있는 위치,\
    즉 가장 최근에 확인하기 시작한 지점.
  * '적용 가능한 동작자'는 할 수 있는 선택,\
    즉 앞으로 간다거나, 오른쪽으로 가거나,\
    왼쪽으로 가는 등의 행동.
* 전체적으로 보면
  * 현재 위치에서\
    할 수 있는 한 방향(동작)을 선택.\
    처음에는 시작점에서 가능한 한 방향 선택.
  * 그 방향으로 한 걸음 진행.\
    이때, '한 걸음 나아간 위치'가\
    '최근에 첨가된 노드'가 됨.
  * 새로운 위치에서\
    다시 할 수 있는 한 방향을 선택하고,\
    그 방향으로 한 걸음 진행.
  * 이런 식으로, 출구를 찾거나\
    더 이상 갈 수 없는 지점(막다른 곳)에\
    도달할 때까지 반복.
  * 만약 막다른 곳에 도달하면,\
    한 단계 뒤로 돌아가 다른 방향을 탐색.

### 장단점

#### 장점

* 현 경로상의 노드들만을 기억하면 되므로\
  저장공간의 수요가 비교적 적다.
* 목표노드가 깊은 단계에 있을 경우\
  해를 빨리 구할 수 있다.

#### 단점

* 해가 없는 경로에 깊이 빠질 가능성이 있다.\
  따라서 실제의 경우\
  미리 지정한 임의의 깊이까지만 탐색하고\
  목표 노드를 발견하지 못하면\
  다음의 경로를 따라 탐색하는 방법이\
  유용할 수 있다.
* 얻어진 해가 최단 경로가 된다는 보장이 없다.\
  이는 목표에 이르는 경로가 다수인 문제에 대해\
  깊이 우선 탐색은 해에 다다르면\
  탐색을 끝내버리므로, 이때 얻어진 해는\
  최적이 아닐 수 있다는 의미이다.

### python

```python
def dfs(graph: dict, start: int):
    # 방문 배열. 
    # visited[node] = True이면 
    # node는 방문이 끝난 상태이다.
    visited = {i: False for i in graph.keys()}  
    stack = [start]
    visited_nodes = []  

    while stack:
        current = stack.pop()
        # current가 방문되지 않았다면
        # current 방문
        if not visited[current]:     
            visited[current] = True  
            # 방문한 노드를 저장
            visited_nodes.append(current)  
             # current의 인접 노드를 확인한다.
            for nxt in graph[current]: 
                # 만일 인접 노드가 
                # 방문되지 않았다면
                # 스택에 추가하여 
                # 나중에 방문할 수 있도록 한다.
                if not visited[nxt]:  
                    stack.append(nxt)  
    return visited_nodes
```

```python
def dfs(graph: dict, start: int):
    visited = {i: False for i in graph.keys()}  
    stack = [start]
    visited_nodes = []  

    while stack:
        current = stack.pop()
        if not visited[current]:     
            visited[current] = True  
            visited_nodes.append(current)  
            for nxt in graph[current]: 
                if not visited[nxt]:  
                    stack.append(nxt)  
    return visited_nodes
```

## 배열 검색

### 2차원 인접배열 근접노드 검색(+)

```python
graph = {}
for i in range(5):
    for j in range(5):
        node = (i, j)
        neighbors = []
        if i > 0:
            neighbors.append((i - 1, j))
        if i < 4:
            neighbors.append((i + 1, j))
        if j > 0:
            neighbors.append((i, j - 1))
        if j < 4:
            neighbors.append((i, j + 1))
        graph[node] = neighbors
```

### 2치원 인접배열 검색 대각선추가 (※)

```python
graph = {}
for i in range(5):
    for j in range(5):
        node = (i, j)
        neighbors = []
        if i > 0:
            neighbors.append((i - 1, j))
        if i < 4:
            neighbors.append((i + 1, j))
        if j > 0:
            neighbors.append((i, j - 1))
        if j < 4:
            neighbors.append((i, j + 1))
        # 대각선 방향의 이웃 노드 추가
        if i > 0 and j > 0:
            neighbors.append((i - 1, j - 1))
        if i < 4 and j < 4:
            neighbors.append((i + 1, j + 1))
        if i > 0 and j < 4:
            neighbors.append((i - 1, j + 1))
        if i < 4 and j > 0:
            neighbors.append((i + 1, j - 1))
        graph[node] = neighbors

for item in graph.items():
    print(item)
```
