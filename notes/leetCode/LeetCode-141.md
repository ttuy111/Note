### [环形链表](https://leetcode-cn.com/problems/linked-list-cycle/description/)  

题目要求：  

给定一个链表，判断链表中是否有环  

进阶：  

不使用额外空间解决  

思考：  

判断链表中是否有环，只要使用两个指针，一个快指针 一个慢指针，指针移动过程中快指针又和慢指针相遇，即可证明链表中是有环，反之没环。

解决代码：  

```
 public boolean hasCycle(ListNode head) {
      if (head==null||head.next==null){
            return false;
        }
        ListNode pre = head;
        ListNode cur = head.next;
        while (pre!=cur){
           if (cur ==null||cur.next ==null){
               return false;
           }
           pre = pre.next;
           cur = cur.next.next;
        }
        return true;
    }
```