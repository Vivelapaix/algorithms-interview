# Linked list

+ [Пересечение двух односвязных списков](#пересечение-двух-односвязных-списков)

## Пересечение двух односвязных списков

Найти узел пересечения двух односвязных списков, имеющих общий конец списков. 

Если такого узла нет, то вернуть null. Переменная end введена для контроля, если списки не пересекаются.

https://leetcode.com/problems/intersection-of-two-linked-lists/

```python
def getIntersectionNode(self, headA, headB):
    if headA == None or headB == None:
        return

    end = 0 
    pA, pB = headA, headB

    while pA != pB:

        if not pA:
            pA = headB
            end = end + 1
        else:
            pA = pA.next

        if not pB:
            pB = headA
            end = end + 1
        else:
            pB = pB.next

        if end > 2:
            break

    return None if end > 2 else pA
```
