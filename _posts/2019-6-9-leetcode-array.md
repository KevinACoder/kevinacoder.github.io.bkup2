---
layout: post
title: LeetCode专题-数组和矩阵
---

## 48. Rotate Image

Medium

You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

Note:

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

```
Example 1:

Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]

Example 2:

Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```
题目大意：将输入矩阵顺时针旋转90度。

解题思路：先将矩阵按行反序，然后交换(x,y)和(y,x)处的元素。

```c++
class Solution {
public:
    /*
    1  2  3                 7  8  9          7  *4  *1

    4  5  6      -->        4  5  6　　 -->  *8  5  *2　　

    7  8  9                 1  2  3　　　　　 *9  *6  3
    reverse by row
    exchange (i, j) with (j, i)
    */
    void rotate(vector<vector<int>>& matrix) {
        reverse(matrix.begin(), matrix.end());
        int rows = (int)matrix.size();
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < i; j++) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }
    }
};
```
测试一下，
```
Success
Details
Runtime: 4 ms, faster than 94.67% of C++ online submissions for Rotate Image.
Memory Usage: 9 MB, less than 71.33% of C++ online submissions for Rotate Image.
```