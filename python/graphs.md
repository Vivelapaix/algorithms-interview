# Graphs

+ [Количество компонент связности](#количество-компонент-связности)

## Количество компонент связности

Найти количество компонент связности неориентированного графа при помощи поиска в глубину.


```python
def connected_components(graph):
    visited = [False] * len(graph)
    cc = 0
    for i in range(len(graph)):
        if not visited[i]:
            dfs(graph, visited, i)
            cc += 1
    return cc


def dfs(graph, visited, v):
    stack = [v]
    while len(stack) > 0:
        current = stack.pop()
        visited[current] = True
        for u in graph[current]:
            if not visited[u]:
                stack.append(u)


v, e = map(int, input().split())
graph = [[] for i in range(v)]

for _ in range(e):
    u, v = map(int, input().split())
    graph[u - 1].append(v - 1)
    graph[v - 1].append(u - 1)

print(connected_components(graph))      
```
