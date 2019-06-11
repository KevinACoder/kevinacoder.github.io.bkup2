---
layout: post
title: LeetCode专题-模拟计算
---

## 目录
419. Battleships in a Board
43. Multiply Strings
54. Spiral Matrix

## 419. Battleships in a Board

Medium

Given an 2D board, count how many battleships are in it. The battleships are represented with 'X's, empty slots are represented with '.'s. You may assume the following rules:

    You receive a valid board, made of only battleships or empty slots.
    Battleships can only be placed horizontally or vertically. In other words, they can only be made of the shape 1xN (1 row, N columns) or Nx1 (N rows, 1 column), where N can be of any size.
    At least one horizontal or vertical cell separates between two battleships - there are no adjacent battleships.

Example:

X..X
...X
...X

In the above board there are 2 battleships.

Invalid Example:

...X
XXXX
...X

This is an invalid board that you will not receive - as battleships will always have a cell separating between them.

Follow up:
Could you do it in one-pass, using only O(1) extra memory and without modifying the value of the board?

题目大意：给一个矩阵作为地图描述船的位置和形状，要求求出船的数目。

解题思路：从上往下，从左往右，数出船的数目。

```c++
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int ans = 0;
        for (int i = 0; i < static_cast<int>(board.size()); i++) {
            for (int j = 0; j < static_cast<int>(board[i].size()); j++) {
                if (board[i][j] == 'X') {
                    if ((i < 1 || board[i - 1][j] == '.') && (j < 1 || board[i][j - 1] == '.')) {
                        ans++;
                    }
                }
            }
        }
        return ans;        
    }
};
```
测试结果，
```
Success
Details
Runtime: 4 ms, faster than 99.66% of C++ online submissions for Battleships in a Board.
Memory Usage: 9.7 MB, less than 45.37% of C++ online submissions for Battleships in a Board.
```

## 43. Multiply Strings

Medium

Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

```
Example 1:

Input: num1 = "2", num2 = "3"
Output: "6"

Example 2:

Input: num1 = "123", num2 = "456"
Output: "56088"
```

> Note:
    > The length of both num1 and num2 is < 110.
    > Both num1 and num2 contain only digits 0-9.
    > Both num1 and num2 do not contain any leading zero, except the number 0 itself.
    > You must not use any built-in BigInteger library or convert the inputs to integer directly.

题目大意：以字符串的形式给定两个数组，求出两数的乘积，以字符串的形式返回。

解题思路：模拟手算两数相乘的过程。注意数字的高位是字符串的低位。注意最后结果字符串中零的处理，去除高位的无效零，但是结果为零时，需要保留最后一位零。

```c++
class Solution {
public:
    string multiply(string num1, string num2) {
        size_t m = num1.size(), n = num2.size();
        string ans;
        int sum = 0;
        vector<int> prod(m + n);
        //for (size_t i = m - 1; i >= 0; i--) {
        //	for (size_t j = n - 1; j >= 0; j--) {
        //		size_t low = i + j + 1,
        //			high = i + j;  //in reverse order
        //		sum = (num1[i] - '0') * (num2[j] - '0') + prod[low];
        //		prod[high] += sum / 10;
        //		prod[low] = sum % 10;
        //	}
        //} //do not use size_t in for loop
        for (int i = m - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                int p1 = i + j;
                int p2 = i + j + 1;
                int sum = (num1[i] - '0') * (num2[j] - '0') + prod[p2];
                prod[p1] += sum / 10;
                prod[p2] = sum % 10;
            }
        }

        //find the highest valid bit
        size_t idx = 0;
        while (idx < prod.size() - 1 && prod[idx] == 0) {
            idx++;
        }

        //combine the results
        while (idx < prod.size()) {
            ans += to_string(prod[idx++]);
        }

        return move(ans);        
    }
};
```
测试一下，
```
Success
Details
Runtime: 4 ms, faster than 98.40% of C++ online submissions for Multiply Strings.
Memory Usage: 9 MB, less than 61.35% of C++ online submissions for Multiply Strings.
```

## 54. Spiral Matrix

Medium

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

```
Example 1:

Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]

Example 2:

Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

题目大意：以顺时针螺旋的方式打印一个矩阵。

解题思路：模拟顺时针读矩阵的过程，方向：右-下-左-上-右...，用四条边界来控制读的停止。

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> ans;
        if(matrix.size() == 0) //empty matrix
            return ans;
        int x = 0, y = 0, d = 0;
        int total = matrix.size()*matrix[0].size();
        int right = matrix[0].size() - 1, left = 0, up = 0, down = matrix.size() - 1;
        //print until all elements have been printed
        while(ans.size() < total){
            switch(d){
                case 0: //to right
                    if(y == right){
                        d = (d+1)%4; //change move direction
                        up++; //the uppest row is removed from next loop of reading
                    }else{
                        ans.push_back(matrix[x][y]);
                        y++; //print and advance
                    }
                    break;
                case 1: //to down
                    if(x == down){
                        d = (d+1)%4;
                        right--;
                    }else{
                        ans.push_back(matrix[x][y]);
                        x++;
                    }
                    break;
                case 2: //to left
                    if(y == left){
                        d = (d+1)%4;
                        down--;
                    }else{
                        ans.push_back(matrix[x][y]);
                        y--;
                    }
                    break;
                case 3: //to up
                    if(x == up){
                        d = (d+1)%4;
                        left++;
                    }else{
                        ans.push_back(matrix[x][y]);
                        x--;
                    }
                    break;
            }
        }
        return move(ans);        
    }
};
```
测试一下，
```
Success
Details
Runtime: 0 ms, faster than 100.00% of C++ online submissions for Spiral Matrix.
Memory Usage: 8.7 MB, less than 40.35% of C++ online submissions for Spiral Matrix.
```