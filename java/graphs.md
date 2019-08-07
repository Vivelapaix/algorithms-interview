# Graphs

+ [Количество компонент связности](#количество-компонент-связности)
+ [Порядок прохождения курсов](#порядок-прохождения-курсов)
+ [Clone Graph](#clone-graph)
+ [Number of Islands](#number-of-islands)
+ [Alien Dictionary](#alien-dictionary)

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

## Clone Graph

https://leetcode.com/problems/clone-graph/

Применить BFS.

```java
public Node cloneGraph(Node node) {
    if (node == null) return null;
    Map<Node, Node> map = new HashMap<>();
    Queue<Node> queue = new ArrayDeque<>();
    Node current;

    map.put(node, new Node(node.val, new ArrayList<Node>()));
    queue.add(node);

    while(!queue.isEmpty()) {
        current = queue.poll();
        for(Node neighbor : current.neighbors) {
            if (!map.containsKey(neighbor)) {
                map.put(neighbor, new Node(neighbor.val, new ArrayList<Node>()));
                queue.add(neighbor);
            }
            map.get(current).neighbors.add(map.get(neighbor));
        }
    }
    return map.get(node);
}
```

## Number of Islands

https://leetcode.com/problems/number-of-islands/

```java
public int numIslands(char[][] grid) {
    int count = 0;
    for (int row = 0; row < grid.length; row++) {
        for (int column = 0; column < grid[0].length; column++) {
            if (grid[row][column] == '1') {
                dfs(grid, row, column);
                count++;    
            }
        }
    }
    return count;
}

private void dfs(char[][] grid, int row, int column) {
    if (row < 0 || row >= grid.length ||
        column < 0 || column >= grid[0].length || 
        grid[row][column] == '0' || grid[row][column] == 'x') {
        return;
    }

    grid[row][column] = 'x';
    dfs(grid, row + 1, column);
    dfs(grid, row - 1, column);
    dfs(grid, row, column + 1);
    dfs(grid, row, column - 1);
}
```

## Alien Dictionary

https://leetcode.com/problems/alien-dictionary/

https://www.geeksforgeeks.org/given-sorted-dictionary-find-precedence-characters/

```java
public String alienOrder(String[] words) {
    if(words == null || words.length == 0) return "";
    Map<Character, Set<Character>> graph = new HashMap<>();
    Map<Character, Integer> inDegree = new HashMap<>();
    buildGraph(words, graph, inDegree);
    String order = topologicalSort(graph, inDegree);
    return order.length() == graph.size() ? order : "";
}

private void buildGraph(String[] words,
                        Map<Character, Set<Character>> graph,
                        Map<Character, Integer> inDegree) {
    for (String s : words) {
        for (char c : s.toCharArray()) {
            graph.put(c, new HashSet<>());
            inDegree.put(c, 0);
        }
    }

    for (int i = 0; i < words.length - 1; i++) {
        String first = words[i];
        String second = words[i + 1];
        int len = Math.max(first.length(), second.length());

        for (int j = 0; j < len; j++) {
            char src = first.charAt(j);
            char dst = second.charAt(j);
            if (src != dst) {
                if (!graph.get(src).contains(dst)) {
                    graph.get(src).add(dst);
                    inDegree.put(dst, inDegree.get(dst) + 1);
                }
                break;
            }
        }
    }
}

private String topologicalSort(Map<Character, Set<Character>> graph,
                               Map<Character, Integer> inDegree) {
    Queue<Character> queue = new ArrayDeque<>();
    for (char c : graph.keySet()) {
        if (inDegree.get(c) == 0) {
            queue.add(c);
        }
    }
    char current;
    StringBuilder order = new StringBuilder();
    while(!queue.isEmpty()) {
        current = queue.poll();
        order.append(current);
        for (char neighbor : graph.get(current)) {
            inDegree.put(neighbor, inDegree.get(neighbor) - 1);
            if (inDegree.get(neighbor) == 0) {
                queue.add(neighbor);
            }
        }
    }
    return order.toString();
}
```
