# Stacks and Queues

+ [Сортировка стека](#сортировка-стека)
+ [Стек с поддержкой минимального элемента](#стек-с-поддержкой-минимального-элемента)

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

## Стек с поддержкой минимального элемента

Реализовать стек с поддержкой возврата минимального элемента за константное время.

```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.Stack;

public class MinStack extends Stack<Integer> {
    private Deque<Integer> minStack;

    public MinStack() {
        minStack = new ArrayDeque<>();
    }

    public void push(int x) {
        if (x <= getMin()) {
            minStack.push(x);
        }
        super.push(x);
    }

    public Integer pop() {
        if (super.peek() == getMin()) {
            minStack.pop();
        }
        return super.pop();
    }

    public int getMin() {
        return minStack.isEmpty() ? Integer.MAX_VALUE : minStack.peek();
    }
}
```

