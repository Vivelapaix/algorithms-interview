# Stacks and Queues

+ [Сортировка стека](#сортировка-стека)

## Сортировка стека

Отсортировать стек, используя другой стек.

**Input:** 4 1 5 2 3 (peek = 3)  
**Output:** 5 4 3 2 1 (peek = 1)

```java
public static void sortStack(Deque<Integer> stack) {
    Deque<Integer> secondStack = new ArrayDeque<>();

    while (!stack.isEmpty()) {
        int firstStackElem = stack.pop();
        while (!secondStack.isEmpty() &&
                secondStack.peek() > firstStackElem) {
            stack.push(secondStack.pop());
        }
        secondStack.push(firstStackElem);
    }

    while (!secondStack.isEmpty()) {
        stack.push(secondStack.pop());
    }
}       
```
