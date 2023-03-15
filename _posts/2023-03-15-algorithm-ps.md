---
layout: post
title: Prefix Sum
subtitle: This is the learning notes for Prefix Sum 
# cover-img: /assets/img/cover1.png
# thumbnail-img: /assets/img/thumbnail1.jpeg
# share-img: /assets/img/cover1.png
tags: [Algorithms]
---

This is a learning note related to the prefix sum (cumulative sum) algorithm. It contains the basic ideas of the prefix sum and some practice examples. 

## Prefix Sum

The prefix sum refers to the sum of all array elements (including itself) before a subscript of an array. The prefix sum is divided into one-dimensional prefix sum and two-dimensional prefix sum. Prefix sum is an important preprocessing, which can reduce the time complexity of the algorithm.

### Practice

1. New a array `preSum` where `preSum [I]` records the cumulative sum of `nums[0 .. i-1]` or other kind of sum
2. Use the `preSum` to reach the answer.

* 303 Range Sum Query - Immutable

  * Question: Given an integer array `nums`, handle multiple queries of the following type:

    1. Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

    Implement the `NumArray` class:

    - `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
    - `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).
  
  * Solution: As basic idea suggested, we need a `preSum` list to help to compute the cumulative sum.
  
  * Code:
  
    ```java
    class NumArray {
        // prefix sum list
        private int[] preSum;
        // construct prefix sum list
        public NumArray(int[] nums) {
            // preSum[0] = 0
            preSum = new int[nums.length+1];
            for(int i = 1; i < preSum.length; i++){
                preSum[i] = preSum[i-1] + nums[i-1];
            }
        }
        // compute the cumulative sum between [left, right]
        public int sumRange(int left, int right) {
            return preSum[right+1] - preSum[left];
        }
    }
  
* 875 Koko Eating Bananas

  * Questions: Given a 2D matrix `matrix`, handle multiple queries of the following type:

    - Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

    Implement the `NumMatrix` class:

    - `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
    - `int sumRegion(int row1, int col1, int row2, int col2)` Returns the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

    You must design an algorithm where `sumRegion` works on `O(1)` time complexity.

  * Solution: Same as previous one, but we need to extend it to 2D matrix. The sum of the elements of the matrix can be converted into the sum of the elements of the matrix around it.

  * Code:

    ```java
    class Solution {
        public int minEatingSpeed(int[] piles, int h) {
            // left and right boundary, left closed right open
            // left boundary means the minimum amount Koko can eat
            // right boundary is one more than the amount of bananas that may in a pile
            int left = 1;
            int right = 1000000000+1;
    
            while(left < right){
                int mid = left + (right - left) / 2;
                if(f(piles, mid) <= h) right = mid;
                else left = mid + 1;
            }
    
            return left;
        }
    
        // auxiliary function
        // where x is k, f(x) is the number of hours, target is h
        public int f(int[] piles, int k){
            int h = 0;
            for(int i = 0; i < piles.length; i++){
                // compute how long it will take Koko to finish eating thi pile
                // be careful, if Koko eats all bananas in a pile, 
                // she will not eat any more bananas during this hour
                h += piles[i] / k;
                if(piles[i] % k != 0) h++;
            }
            return h;
        }
    }
    ```

    