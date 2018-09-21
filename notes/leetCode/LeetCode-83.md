### [删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/description/)

题目要求：  

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。  

示例：  

```
输入: 1->1->2
输出: 1->2
```  

思考：  

看到这种题目首先想到的就是判断 next 是否和当前 val 相等，如果相等，就把指针 next 指向 next.next（此时不用判断空，待下一 while 循环时判断即可）直到把整个链表循环一遍，便会剔除重复项。

解决代码：

```
public ListNode deleteDuplicates(ListNode head) {

         if (head == null||head.next==null) return head;
        ListNode temp = head;
        while (temp!=null&&temp.next!=null){
            if (temp.val == temp.next.val){
                temp.next = temp.next.next;
            }else {
                temp = temp.next;
            }
        }
        return head;
    }
```