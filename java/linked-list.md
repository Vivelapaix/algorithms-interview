# Linked list

+ [Повернуть односвязный список](#повернуть-односвязный-список)
+ [Пересечение двух односвязных списков](#пересечение-двух-односвязных-списков)
+ [Найти цикл в односвязном списке](#цикл-в-односвязном-списке)
+ [Найти цикл в односвязном списке 2](#цикл-в-односвязном-списке-2)
+ [Палиндром односвязный список](#палиндром-односвязный-список)
+ [Remove Nth Node From End of List](#remove-nth-node-from-end-of-list)


## Повернуть односвязный список в обратном порядке

```java
public void reverse() {
    Node previousNode = null;
    Node currentNode = head;
    Node nextNode;

    while (currentNode != null) {
        nextNode = currentNode.next;
        currentNode.next = previousNode;
        previousNode = currentNode;
        currentNode = nextNode;
    }
    head = previousNode;
}

or

public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;

    ListNode prev = null;
    ListNode cur = head;
    ListNode next;

    while (cur != null) {
        next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    return prev;
}
```

Идея в том, чтобы хранить 3 указателя: предыдущий элемент, текущий и следующий элементы. На каждой итерации сначала получаем
следующий элемент, затем у текущего элемента указатель next перебрасываем на предыдущий элемент, затем текущий элемент становится предыдущим,
а следующий элемент становится текущим, далее новая итерация.

```java
public class MyLinkedList<E>{

    private Node head;

    static class Node<E> {
        E value;
        Node next;
        Node(E value) {
            this.value = value;
        }
    }

    public void add(E elem) {
        MyLinkedList.Node newNode = new MyLinkedList.Node(elem);

        if (head == null) {
            head = newNode;
        } else {
            Node current = head;
            while (current.next != null)
                current = current.next;
            current.next = newNode;
        }
    }

    public void print() {
        Node currentNode = head;

        while (currentNode != null) {
            System.out.println(currentNode.value);
            currentNode = currentNode.next;
        }
    }

    public void reverse() {
        Node previousNode = null;
        Node currentNode = head;
        Node nextNode;

        while(currentNode != null) {
            nextNode = currentNode.next;
            currentNode.next = previousNode;
            previousNode = currentNode;
            currentNode = nextNode;
        }
        head = previousNode;
    }
}      
```

## Пересечение двух односвязных списков

Найти узел пересечения двух односвязных списков, имеющих общий конец списков. 

Если такого узла нет, то вернуть null. Переменная end введена для контроля, если списки не пересекаются.

https://leetcode.com/problems/intersection-of-two-linked-lists/

```java
public static ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) return null;

    int end = 0;
    ListNode pA = headA;
    ListNode pB = headB;

    while (pA != pB) {
        if (pA == null) {
            pA = headB;
            end++;
        } else {
            pA = pA.next;
        }

        if (pB == null) {
            pB = headA;
            end++;
        } else {
            pB = pB.next;
        }

        if (end > 2) {
            break;
        }
    }

    return end > 2 ? null : pA;
}
```


## Найти цикл в односвязном списке

Дан односвязный список, вернуть узел начала цикла в списке. Если нет цикла, то вернуть null.

https://leetcode.com/problems/linked-list-cycle/
https://leetcode.com/problems/linked-list-cycle-ii/
https://java2blog.com/find-start-node-of-loop-in-linkedlist/

```java
public static ListNode detectCycle(ListNode head) {
    if (head == null) return null;

    ListNode slow = head;
    ListNode fast = head;

    while (true) {
        if (fast == null || fast.next == null) {
            return null;
        }

        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) {
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;
        }
    }
}
```

## Найти цикл в односвязном списке 2

Дан односвязный список. Определеить, существует ли цикл в данном списке.

https://leetcode.com/problems/linked-list-cycle/

```java
public boolean hasCycle(ListNode head) {
    if (head == null) return false;

    ListNode slow = head;
    ListNode fast = head;

    while (true) {
        if (fast == null || fast.next == null) {
            return false;
        }

        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) {
            return true;
        }
    }
}
```

## Палиндром односвязный список

Проверить, является ли односвязный список палиндромом.

https://leetcode.com/problems/palindrome-linked-list/

```java
public ListNode reverseList(ListNode head) {
    if (head == null) return head;

    ListNode prev = null;
    ListNode cur = head;
    ListNode next;

    while (cur != null) {
        next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    return prev;
}

public boolean isEqualLists(ListNode firstList, ListNode secondList) {
    while (firstList != null && secondList != null) {
        if (firstList.val != secondList.val) {
            return false;
        }
        firstList = firstList.next;
        secondList = secondList.next;
    }
    return true;
}

public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) {
        return true;
    }

    ListNode slow = head;
    ListNode fast = head;

    while (true) {
        if (fast == null || fast.next == null) {
            break;
        }

        slow = slow.next;
        fast = fast.next.next;
    }

    if (fast != null) {
        slow = slow.next;
    }

    slow = reverseList(slow);

    return isEqualLists(head, slow);
}
```

## Remove Nth Node From End of List

https://leetcode.com/problems/remove-nth-node-from-end-of-list/

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    if (head == null || n == 0) return head;
    ListNode first = head, second = head;

    while (n-- > 0) {
        first = first.next;
    }

    while (first != null && first.next != null) {
        first = first.next;
        second = second.next;
    }

    first = first == null ? second : second.next;
    if (first == head) {
        head = first.next;
    } else {
        second.next = first.next;
    }
    first.next = null;

    return head;
}
```
