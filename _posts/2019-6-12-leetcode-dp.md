---
layout: post
title: LeetCode专题-动态规划
---

## 目录

55. Jump Game
45. Jump Game II

## 55. Jump Game

Medium

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

```
Example 1:

Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

Example 2:

Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```

题目大意：给一个数组，数组中的每一个元素表示从当前可以向后跳的步数，要求求出可否从第一步跳到最后一步。

解题思路：定义一个转移方程，因为要到当前位置，跳跃要不就从前一步发生，要不就从更前的某位发生，完成跳跃需要花费1。用dp记录跳到某步时，依然拥有往后再跳几步的能力。

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        if(nums.size() == 0)
            return true;

        vector<int> dp(nums.size(), 0);
        dp[0] = max(0, nums[0]); //steps we still have after the step 0
        for(int i = 1; i < nums.size(); i++){
            if(dp[i-1] < 0)
                return false;

            //steps we have after step i, we can either jump from prev-prev step
            //  or prev step
            dp[i] = max(dp[i-1], nums[i-1]) - 1;
        }

        return dp.back() >= 0; //need to have step available       
    }
};
```
测试一下，
```
Success
Details
Runtime: 8 ms, faster than 98.62% of C++ online submissions for Jump Game.
Memory Usage: 10 MB, less than 51.25% of C++ online submissions for Jump Game.
```

## 45. Jump Game II

Hard

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

```
Example:

Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

Note:
You can assume that you can always reach the last index.

题目大意：接上题，假设一定能跳到终点，要求求出跳到终点需要的最少步数。

解题思路：同样用动态规划，dp数组用来存跳到当前节点需要的最少步数。

```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        if(nums.size() == 0)
            return 0;

        // dp contains the jump needed to reache current step
        vector<int> dp(nums.size(), INT_MAX);
        dp[0] = 0; //need 0 jump to reach step 1

        for(int i = 0; i < nums.size(); i++){
            //from step i, one is able to jump to step i + nums[i]
            int j = min(i + nums[i], (int)nums.size()-1);
            //update jump num in [i:j]
            for(; j >= i; j--){
                //if step j is already reachable in previous jump,
                //  the current jump cannot have less jump num, therefore break out
                if(dp[j] < INT_MAX){
                    break;
                }
                dp[j] = dp[i] + 1; //update jump num
            }
        }

        return dp.back();        
    }
};
```
测试一下，
```
Success
Details
Runtime: 12 ms, faster than 88.80% of C++ online submissions for Jump Game II.
Memory Usage: 10.4 MB, less than 47.05% of C++ online submissions for Jump Game II.
```