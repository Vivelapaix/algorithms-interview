# Linked list

+ [Reverse Linked List](#reverse-linked-list)
+ [Intersection of Two Linked Lists](#intersection-of-two-linked-lists)
+ [Linked List Cycle](#linked-list-cycle)
+ [Linked List Cycle II](#linked-list-cycle-ii)
+ [Palindrome Linked List](#palindrome-linked-list)
+ [Remove Nth Node From End of List](#remove-nth-node-from-end-of-list)
+ [Reorder List](#reorder-list)
+ [Middle of the Linked List](#middle-of-the-linked-list)
+ [Merge Two Sorted Lists](#merge-two-sorted-lists)
+ [Sort List](#sort-list)
+ [Add Two Numbers](#add-two-numbers)
+ [Merge k Sorted Lists](#merge-k-sorted-lists)
+ [Remove Duplicates from Sorted List II](#remove-duplicates-from-sorted-list-ii)


## Reverse Linked List

https://leetcode.com/problems/reverse-linked-list/

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
    ListNode prev = null;
    ListNode cur = head;
    ListNode next = null;

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

## Intersection of Two Linked Lists

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

## Linked List Cycle

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
Пояснения:
```
1) When slow pointer enters the loop, the fast pointer must be inside the loop. Let fast pointer be distance k from slow.

2) Now if consider movements of slow and fast pointers, we can notice that distance between them (from slow to fast) increase by one after every iteration. After one iteration (of slow = next of slow and fast = next of next of fast), distance between slow and fast becomes k+1, after two iterations, k+2, and so on. When distance becomes n, they meet because they are moving in a cycle of length n.
```

## Linked List Cycle II

Дан односвязный список, вернуть узел начала цикла в списке. Если нет цикла, то вернуть null.

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


## Palindrome Linked List

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

## Reorder List

https://leetcode.com/problems/reorder-list/

Развернуть вторую половину списка и слить в один список.

```java
private ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;

    ListNode cur = head;
    ListNode prev = null;
    ListNode next;

    while (cur != null) {
        next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    return prev;
}

private ListNode mergeLists(ListNode first, ListNode second) {
    ListNode curFirst = first, nextFirst;
    ListNode curSecond = second, nextSecond;

    while (curFirst != null && curSecond != null) {
        nextFirst = curFirst.next;
        nextSecond = curSecond.next;

        curFirst.next = curSecond;
        curSecond.next = nextFirst;

        curFirst = nextFirst;
        curSecond = nextSecond;
    }
    return first;
}

public void reorderList(ListNode head) {
    if (head == null || head.next == null || head.next.next == null) {
        return;
    }

    ListNode slow = head;
    ListNode fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    fast = slow;
    if (fast != null) {
        fast = slow.next;
        slow.next = null;
    }

    fast = reverseList(fast);        
    head = mergeLists(head, fast);
}
```

## Middle of the Linked List

https://leetcode.com/problems/middle-of-the-linked-list/

```java
public ListNode middleNode(ListNode head) {
    if (head == null || head.next == null) return head;

    ListNode slow = head;
    ListNode fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

## Merge Two Sorted Lists

https://leetcode.com/problems/merge-two-sorted-lists/

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if (l1 == null && l2 == null) return null;
    if (l1 == null) return l2;
    if (l2 == null) return l1;

    ListNode sortedList = new ListNode(0);
    ListNode cur = sortedList;

    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            cur.next = l1;
            l1 = l1.next;
        } else {
            cur.next = l2;
            l2 = l2.next;
        }
        cur.next.next = null;
        cur = cur.next;
    }

    if (l1 != null) cur.next = l1;
    if (l2 != null) cur.next = l2;

    return sortedList.next;
}
```

## Sort List

https://leetcode.com/problems/sort-list/

```java
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) return head;

    ListNode slow = head;
    ListNode fast = head;
    ListNode prevSlow = head;

    while (fast != null && fast.next != null) {
        prevSlow = slow;
        slow = slow.next;
        fast = fast.next.next;
    }

    if (fast != null) {
        prevSlow = slow;
        slow = slow.next;
    }
    prevSlow.next = null;

    return mergeLists(sortList(head), sortList(slow));
}

private ListNode mergeLists(ListNode l1, ListNode l2) {
    if (l1 == null && l2 == null) return null;
    if (l1 == null) return l2;
    if (l2 == null) return l1;

    ListNode head = new ListNode(0);
    ListNode cur = head;

    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            cur.next = l1;
            l1 = l1.next;
        } else {
            cur.next = l2;
            l2 = l2.next;
        }
        cur.next.next = null;
        cur = cur.next;
    }

    if (l1 != null) cur.next = l1;
    if (l2 != null) cur.next = l2;

    return head.next;
}
```

## Add Two Numbers

https://leetcode.com/problems/add-two-numbers/

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode();
    ListNode current = dummy;
    int carry = 0;

    while (l1 != null || l2 != null) {

        int val1 = l1 != null ? l1.val : 0;
        int val2 = l2 != null ? l2.val : 0;

        int sum = val1 + val2 + carry;
        carry = 0;

        if (sum <= 9) {
            current.next = new ListNode(sum);
        } else {
            current.next = new ListNode(sum % 10);
            carry = 1;
        }

        if (l1 != null) {
            l1 = l1.next;
        }
        if (l2 != null) {
            l2 = l2.next;
        }
        current = current.next;
    }

    if (carry != 0) {
        current.next = new ListNode(carry);
    }

    return dummy.next;
}
```


## Merge k Sorted Lists

https://leetcode.com/problems/merge-k-sorted-lists/

```java
public ListNode mergeKLists(ListNode[] lists) {
    int k = lists.length;
    int interval = 1;

    while (interval < k) {
        for (int i = 0; i < k - interval; i += interval * 2) {
            lists[i] = mergeTwoLists(lists[i], lists[i + interval]);
        }
        interval *= 2;
    }

    return k > 0 ? lists[0] : null;
}

private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode();
    ListNode current = dummy;

    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current.next.next = null;
        current = current.next;
    }

    if (l1 != null) {
        current.next = l1;
    }

    if (l2 != null) {
        current.next = l2;
    }

    return dummy.next;
}
```

Пояснения:
```
Разделяй и властвуй

[[1,4,5],[1,3,4],[2,6],[1,2,5],[4,5]]
Сначала берем и мержим по индексам [0, 1] [2, 3] [4] -> 0, 2, 4
                                   [0, 2] [4]        -> 0, 4
                                   [0, 4]            -> 0
Таким образом нам нужна переменная интервал, которая с каждой интерацией увеличивается в 2 раза, потому что сначала мержили индексы,
которые на расстоянии 1, потом на расстоянии 2, потом на расстоянии 4
```

## Remove Duplicates from Sorted List II

https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/

```java
public ListNode deleteDuplicates(ListNode head) {
    ListNode dummy = new ListNode(-1, head);
    ListNode cur = dummy;
    while (cur != null) {
        ListNode lastDuplicate = passDuplicates(cur.next);
        if (lastDuplicate == cur.next) {
            cur = cur.next;
        } else {
            cur.next = lastDuplicate.next;
        }
    }
    return dummy.next;
}

public ListNode passDuplicates(ListNode head) {
    ListNode cur = head, prev = null;
    while (cur != null && cur.val == head.val) {
        prev = cur;
        cur = cur.next;
    } 
    return prev;
}
```
