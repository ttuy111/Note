### [两数之和](https://leetcode-cn.com/problems/two-sum/description/)  

题目要求：  

给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。  

示例：  

```
给定 nums = [2, 7, 11, 15], target = 9


因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

思考：  

求两个数值，那么只要把和减去数组中的某个值，看另外一个值是否在数组中即可。为了寻找这组数据，可以把遍历到的数值存到某个数据结构中，这样只要看 `target-nums[i]` 的值是否在这个数据结构中即可。查看某个数是否在某种数据结构中，可以利用 `HashMap` 的 API方法 `containsKey` 来判断是否存在当前数值。  

解决代码：

```
  public int[] twoSum(int[] nums, int target) {
          HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i])) {
                return new int[]{map.get(target - nums[i]), i};
            } else {
                map.put(nums[i], i);
            }
        }
        return null;
    }

```