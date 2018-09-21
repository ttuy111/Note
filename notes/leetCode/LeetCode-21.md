### [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/description/)  

题目要求：  

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。  

示例：

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```  

思考：  

题目要求合成之后还是有序的列表，所以在合成的时 要进行两个链表节点的大小判断。 先判断空 如果 l1 空，返回 l2，l2 空返回 l1，然后对两个链表节点进行对比，把数值大的设为当前节点的值，数值小的和另外一个链表的下一个节点进行对比，运用递归的方法，把两个链表对比完后就能生成一个合成好的新的有序节点。


解决代码：

```
递归方法
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;

        ListNode head = null;
        if (l1.val <= l2.val){
            head = l1;
            head.next = mergeTwoLists(l1.next, l2);
        } else {
            head = l2;
            head.next = mergeTwoLists(l2.next, l1);
        }
        return head;
    }
}
```

还有一种非递归的方法，使用 while 循环，判空后先创建一个根节点，然后将 result 指向此节点，留做返回结果时使用，因为两个链表的长度可能不一样，所以在循环判断时要判断其中一条链表是否已经为空，当两条链表都为空时跳出 while 循环。在循环体中若 l1 为空，那直接把临时节点的下一个节点指向 l2，然后 再把 l2 链表的指针后移一位，以此类推，如果两个节点都为空后，结束循环，此时 result 的指针指向链表头部，但是链表头部节点是我们自己创建的，所以返回的时候需要返回头部节点的下一个节点即 result.next。

解决代码如下：

```
 public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        ListNode tempListNode = new ListNode(0);
        ListNode result = tempListNode;
        while (l1!=null||l2!=null){
            if (l1!=null&&(l2==null||l1.val<l2.val)) {
                tempListNode.next = l1;
                l1 = l1.next;
            }else {
                tempListNode.next = l2;
                l2 = l2.next;
            }
            tempListNode = tempListNode.next;
        }
        return result.next;
    }
```