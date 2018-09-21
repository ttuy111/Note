### [删除链表中的节点（删除给定值的所有节点）](https://leetcode-cn.com/problems/remove-linked-list-elements/description/)

题目要求：

删除链表中等于给定值 `val` 的所有节点。  

示例：  

```
输入: 1->2->6->3->4->5->6, val = 6
输出: 1->2->3->4->5
```  

思考：  

删除指定节点，可以可以理解为将 `head.next.next` 节点指向 `head.next`，理解了这点那么问题就能很轻松的解决。 

解决代码：  

```
 public ListNode removeElements(ListNode head, int val) {
        if (head ==null) return head;
        ListNode resultListNode = new ListNode(0);
        resultListNode.next = head;
        head = resultListNode;
        while (head.next!=null){
            if (head.next.val ==val){
                head.next = head.next.next;
            }else {
                head = head.next;
            }
        }
        return resultListNode.next;
    }
```