# Linked List

用一个 `dummy` 指针，来将头指针的情况统一。

#### 147

```java
public ListNode insertionSortList(ListNode head) {
    if (head == null) {
        return null;
    }
    ListNode dummy = new ListNode(0);
    //拿出的节点
    while (head != null) {
        ListNode tempH = dummy;
        ListNode headNext = head.next;
        head.next = null; 
        while (tempH.next != null) {
            //找到大于要插入的节点的位置
            if (tempH.next.val > head.val) {
                head.next = tempH.next;
                tempH.next = head; 
                break;
            }
            tempH = tempH.next;
        }
        //没有执行插入，将当前节点加到末尾
        if (tempH.next == null) {
            tempH.next = head;
        }
        head = headNext;
    }
    return dummy.next;
}

```

## Fast and Slow pointer

#### 148

{% tabs %}
{% tab title="Java" %}
```java
// step 1. cut the list to two halves
ListNode prev = null, slow = head, fast = head;

while (fast != null && fast.next != null) {
    prev = slow;
    slow = slow.next;
    fast = fast.next.next;
}
```
{% endtab %}

{% tab title="Merge sort" %}
```java
public ListNode sortList(ListNode head) {
    return mergeSort(head);
}

private ListNode mergeSort(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode fast = dummy;
    ListNode slow = dummy;
    //快慢指针找中点
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    ListNode head2 = slow.next;
    slow.next = null;
    head = mergeSort(head);
    head2 = mergeSort(head2);
    return merge(head, head2);

}

private ListNode merge(ListNode head1, ListNode head2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    while (head1 != null && head2 != null) {
        if (head1.val < head2.val) {
            tail.next = head1;
            tail = tail.next;
            head1 = head1.next;
        } else {
            tail.next = head2;
            tail = tail.next;
            head2 = head2.next;
        }

    }
    if (head1 != null) {
        tail.next = head1;
    }

    if (head2 != null) {
        tail.next = head2;
    }

    return dummy.next;

}

```
{% endtab %}
{% endtabs %}

