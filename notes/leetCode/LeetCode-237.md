### [删除链表中的节点（设计方法删除节点）](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/description/)

题目要求：  

请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点，你将只被给定要求被删除的节点。  

示例：  

```
输入: head = [4,5,1,9], node = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
```

说明：  

- 链表至少包含两个节点。
- 链表中所有节点的值都是唯一的。
- 给定的节点为非末尾节点并且一定是链表中的一个有效节点。
- 不要从你的函数中返回任何结果  
  
思考：  
题干中告诉我们 链表至少为两个节点，且给定的节点一定不是末尾节点，则 node.next 一定不为空，并且只给了当前节点，所以可以用next节点覆盖掉当前节点即可。

解决代码：  

```
  public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
```