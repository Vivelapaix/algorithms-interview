# Graphs

+ [Количество компонент связности](#количество-компонент-связности)

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
