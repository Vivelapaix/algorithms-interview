# Graphs

+ [Количество компонент связности](#количество-компонент-связности)
+ [Порядок прохождения курсов](#порядок-прохождения-курсов)

## Количество компонент связности

Найти количество компонент связности неориентированного графа при помощи поиска в глубину.

```java
import java.util.*;

public class ConnectedComponents {

    private static void reach(ArrayList<Integer>[] adj, boolean[] visited, int x) {
        Deque<Integer> stackOrder = new ArrayDeque<>();
        stackOrder.add(x);

        while (stackOrder.size() > 0) {
            int currentVertex = stackOrder.pop();
            visited[currentVertex] = true;

            Iterator<Integer> curElemAdj = adj[currentVertex].iterator();
            while (curElemAdj.hasNext()) {
                int adjElemForCurElem = curElemAdj.next();
                if (visited[adjElemForCurElem] == false) {
                    stackOrder.push(adjElemForCurElem);
                }
            }
        }
    }

    private static int numberOfComponents(ArrayList<Integer>[] adj) {
        int result = 0;
        boolean[] visited = new boolean[adj.length];
        for (int vertex = 0; vertex < adj.length; vertex++) {
            if (visited[vertex] == false) {
                reach(adj, visited, vertex);
                result++;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int m = scanner.nextInt();
        ArrayList<Integer>[] adj = (ArrayList<Integer>[])new ArrayList[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<Integer>();
        }
        for (int i = 0; i < m; i++) {
            int x, y;
            x = scanner.nextInt();
            y = scanner.nextInt();
            adj[x - 1].add(y - 1);
            adj[y - 1].add(x - 1);
        }
        System.out.println(numberOfComponents(adj));
    }
}       
```

## Порядок прохождения курсов

Метки WHITE, GRAY, BLACK используются для поиска цикла при прохождении курсов. GRAY означает, что вершина находится в процессе обработки, то есть принадлежит уже какому-то пути прохождения курсов.

Основная идея в том, что идем с помощью DFS до всех таких вершин, где больше либо нет исходящих направлений, либо всех нужные курсы уже пройдены до данного курса. 

https://leetcode.com/problems/course-schedule-ii/

```java
public class Solution {
    private int WHITE = 0;
    private int GRAY = 1;
    private int BLACK = 2;
    private boolean isPossible;

    public void dfs(Map<Integer, List<Integer>> adjList,
                    int[] color,
                    int x,
                    List<Integer> order) {
        Deque<Integer> stackOrder = new ArrayDeque<>();
        boolean isVertexSink;
        int currentVertex;
        stackOrder.push(x);

        while (!stackOrder.isEmpty()) {
            currentVertex = stackOrder.peek();

            isVertexSink = true;

            if (adjList.containsKey(currentVertex)) {
                for (Integer neighbour : adjList.get(currentVertex)) {
                    if (color[neighbour] == GRAY) {
                        isPossible = false;
                        return;
                    } else if (color[neighbour] == WHITE) {
                        stackOrder.push(neighbour);
                        color[neighbour] = GRAY;
                        isVertexSink = false;
                        break;
                    }
                }
            }

            if (isVertexSink) {
                color[currentVertex] = BLACK;
                stackOrder.pop();
                order.add(currentVertex);
            }
        }
    }

    public int[] findOrder(int numCourses, int[][] prerequisites) {
        Map<Integer, List<Integer>> adjList = new HashMap<>();
        List<Integer> neighbours;
        int src, dst;

        for (int i = 0; i < prerequisites.length; i++) {
            src = prerequisites[i][1];
            dst = prerequisites[i][0];

            neighbours = adjList.getOrDefault(src, new ArrayList<>());
            neighbours.add(dst);
            adjList.put(src, neighbours);
        }

        int[] color = new int[numCourses];
        List<Integer> order = new ArrayList<>();

        isPossible = true;
        for (int course = 0; course < numCourses && isPossible; course++) {
            if (color[course] == WHITE) {
                dfs(adjList, color, course, order);
            }
        }

        if (isPossible && order.size() == numCourses) {
            int[] resOrder = new int[numCourses];
            for (int i = 0; i < numCourses; i++) {
                resOrder[i] = order.get(numCourses - 1 - i);
            }
            return resOrder;
        }
        return new int[0];
    }
}

```
