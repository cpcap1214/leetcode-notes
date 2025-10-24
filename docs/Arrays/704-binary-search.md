---
title: "704. Binary Search"
---

# 704. Binary Search

[題目連結](https://leetcode.com/problems/binary-search/description/)

## 題目

> Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.  
> You must write an algorithm with O(log n) runtime complexity.

## 思路

Binary search 最重要的是什麼時候要 break 掉迴圈，以及 right, left 怎麼更新。

當初我第一次寫的時候也是想半天，一直在 `right = mid` 跟 `mid - 1` 之間不斷嘗試，最後雖然試出來了，但是也不知道為什麼是這樣。

其實只要先提前瞭解、固定你取的區間就可以了。

這是什麼意思呢？一般來說我們取區間會取：左閉右閉、左閉右開這兩種，對應到數學符號就是 `[left, right]` 跟 `[left, right)`，而我們只要選取其中一種區間，往後的程式就按照那個區間就可以了。

### 左閉右閉

由於是左閉右閉，因此像是 `[1, 1]` 這種區間是合法的，因此在 while 迴圈中就要寫到 `left <= right`。

如果沒有那個等號會發生什麼事？遇到 `[1, 1]` 區間的話會直接 break 掉，不會去查 `nums[1]` 是否是我們要的值，所以這樣是錯的。

right 指針更新的部分，因為我們在查 `nums[mid]` 的時候就已經知道這個數字不可能是我們要的，所以我們下一次在檢查的時候就不用檢查這個數字，因此要寫 `right = mid - 1`。

如果寫 `right = mid` 的話，因為更新後的區間是 `[left, mid]`，但是我們已經知道不可能是 `nums[mid]`，這樣會讓程式出錯，所以不用再檢查一次。

### 左閉右開

一樣以 `[1, 1)` 為例，這個區間是不合法的，不可能等於 1 又小於 1，所以在 while 的迴圈就要寫 `left < right`。

同理，我們在查 `nums[mid]` 的時候知道它不可能是答案，所以我們就要用 `[left, mid)` 的區間來做下一次的檢查。

## Code

以下的 code 為左閉右閉：

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(target > nums[mid]) {
                left = mid + 1;
            }
            else if(target < nums[mid]) {
                right = mid - 1;
            }
            else {
                return mid;
            }
        }
        return -1;
    }
};
```