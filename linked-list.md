# Linked list

+ [Повернуть односвязный список](#повернуть-односвязный-список)

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
