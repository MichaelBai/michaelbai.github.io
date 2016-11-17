---
layout:     post
title:      "LeetCode 3Sum"
date:       2016-11-03
author:     "Michael"
header-img: "img/post-bg-2015.jpg"
tags:
    - Algorithm
---

LeetCode 3Sum:

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

**Note**: The solution set must not contain duplicate triplets.

```
For example, given array S = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

基本思路是变3Sum为2Sum问题，代码如下：

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int> > result;
        
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            // 2Sum
            int start = i + 1, end = nums.size() - 1; // trick
            int target = -nums[i];
            while (start < end) {
                if (nums[start] + nums[end] < target) {
                    start++;
                } else if (nums[start] + nums[end] > target) {
                    end--;
                } else {
                    vector<int> triple;
                    triple.push_back(nums[i]);
                    triple.push_back(nums[start]);
                    triple.push_back(nums[end]);
                    result.push_back(triple);
                    start++;
                }
            }
        }
        
        return result;
    }
};
```

上面注释trick的部分，即start=i+1，2Sum的起始点从i的下一个开始，是因为现在数组已经排好序了，假设存在i，start，end，使得nums[i]，nums[start]，nums[end]的和为0，并且start小于i小于end，那么result中肯定已经存在nums[start]为起始元素的一组值，所以这是一步提前去重的trick。
由于数字可以重复出现，所以还有其他类型的重复：比如[-3, -3, 1, 2]，这个时候需要将nums[i]==nums[i-1]的情况去除，代码如下：

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int> > result;
        
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            // 去重1
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            // two sum;
            ...
        }
        
        return result;
    }
};
```

还有另外一种重复的情况[-3,-3,-3,6]，这个时候会生成两个[-3,-3,6]的数组，所以需要在2Sum的时候判断nums[start]是否与前面一个数字相等，以去除此种重复，最终完整代码如下：

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int> > result;
        
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            // two sum;
            int start = i + 1, end = nums.size() - 1;
            int target = -nums[i];
            while (start < end) {
                if (start > i + 1 && nums[start - 1] == nums[start]) {
                    start++;
                    continue;
                }
                if (nums[start] + nums[end] < target) {
                    start++;
                } else if (nums[start] + nums[end] > target) {
                    end--;
                } else {
                    vector<int> triple;
                    triple.push_back(nums[i]);
                    triple.push_back(nums[start]);
                    triple.push_back(nums[end]);
                    result.push_back(triple);
                    start++;
                }
            }
        }
        
        return result;
    }
};
```

参考：
[http://blog.gainlo.co/index.php/2016/07/19/3sum/?utm_source=forum&utm_medium=comment&utm_campaign=forum](http://blog.gainlo.co/index.php/2016/07/19/3sum/?utm_source=forum&utm_medium=comment&utm_campaign=forum)
[http://www.sigmainfy.com/blog/summary-of-ksum-problems.html](http://www.sigmainfy.com/blog/summary-of-ksum-problems.html)
[http://www.jiuzhang.com/solutions/3sum/](http://www.jiuzhang.com/solutions/3sum/)


