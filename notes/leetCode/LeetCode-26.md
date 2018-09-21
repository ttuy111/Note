### [删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/description/)  

因为题目要求必须在**原数组**上进行修改，并且只能使用 ***O(1)*** 的额外空间，所以不能用创建新数组的方式保存处理过得数组的数据。  

 示例：  

`nums = [0,0,1,1,1,2,2,3,3,4] ==》[0,1,2,3,4]`  

思考：  

由示例可知，想把 nums 数组从左边转换成右边，设 pos 为当前新的下标，即”新数组的长度” i 为循环时数组的下标，我们只需判断 `nums[pos] ！= nums[i]` ，当 pos 和 i 不相等时，把当前 i 的数据 挪动到 pos+1 的位置即可，以此类推，这样循环一遍数组之后，我们就可以把不同的数据挪动到数组的前边了。 

解决代码：   

```
双指针方法
class Solution {
    public int removeDuplicates(int[] nums) {
    if (nums==null||nums.length==0) return 0;
             int pos = 0;
            for (int i = 0; i < nums.length; i++) {
                if (nums[pos] != nums[i]){
                 pos++;
                 nums[pos] = nums[i];
                }
            }
            return pos+1;  
    }
}
```