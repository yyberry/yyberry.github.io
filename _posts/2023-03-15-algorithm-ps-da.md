---
layout: post
title: Prefix Sum and Difference Array
subtitle: This is the learning notes for Prefix Sum and Difference Array
# cover-img: /assets/img/cover1.png
# thumbnail-img: /assets/img/thumbnail1.jpeg
# share-img: /assets/img/cover1.png
tags: [Algorithms]
---

This is a learning note related to the prefix sum (cumulative sum) and difference array algorithm. It contains the basic ideas of them and some practice examples. 

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

    

## Difference Array

The prefix sum mainly applicates in such scenario that the cumulative sum of a certain interval is frequently queried when the original array is not modified. While the main application scenario of difference array is to **frequently increase or decrease the elements of a certain interval of the original array**.

### Practice

1. New a array `diff` where `diff [I]` records the difference between `num[i]` and `num[i-1]`
2. Use the `diff` to reach the answer.

* 1094 Car Pooling

  * Question: There is a car with `capacity` empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west).

    You are given the integer `capacity` and an array `trips` where `trips[i] = [numPassengersi, fromi, toi]` indicates that the `ith` trip has `numPassengersi` passengers and the locations to pick them up and drop them off are `fromi` and `toi` respectively. The locations are given as the number of kilometers due east from the car's initial location.

    Return `true` *if it is possible to pick up and drop off all passengers for all the given trips, or* `false` *otherwise*.

  * Solution: Assume we have an array `nums` which records the number of passengers for each stop. Then the `trips` indicates the elements in `nums` is frequently increased or decreased, so we can use difference array to solve this problem. Use `diff` record the difference of the number of passengers for each stop.

  * Code:

    ```java
    class Solution {
        public boolean carPooling(int[][] trips, int capacity) {
          	// define the difference array
            int[] diff = new int[1001];
            for(int i = 0; i<trips.length; i++){
                int[] trip = trips[i];
                int n = trip[0];
                int start = trip[1];
                int end = trip[2];
                diff[start] += n;
                if(end < diff.length)   diff[end] -= n;
            }
    				
          	// build the ressult list by difference array
            int[] res = new int[1001];
            for(int j = 0; j < res.length; j++){
                if(j==0)    res[0]=diff[0];
                else    res[j] = res[j-1] + diff[j];
                if(res[j]>capacity) return false;
            }
            return true;
        }
    }
    ```

* 1109 Corporate Flight Bookings

  * Question: There are `n` flights that are labeled from `1` to `n`.

    You are given an array of flight bookings `bookings`, where `bookings[i] = [firsti, lasti, seatsi]` represents a booking for flights `firsti` through `lasti` (**inclusive**) with `seatsi` seats reserved for **each flight** in the range.

    Return *an array* `answer` *of length* `n`*, where* `answer[i]` *is the total number of seats reserved for flight* `i`.

  * Solution: Same as previous one but return the result list.

  * Code:

    ```java
    class Solution {
        public int[] corpFlightBookings(int[][] bookings, int n) {
            int[] diff = new int[n+1];
            for(int[] booking : bookings){
                int start = booking[0];
                int end = booking[1];
                int seat = booking[2];
                diff[start] += seat;
                if(end+1<diff.length) diff[end+1] -= seat;
            }
    
            int[] answer = new int[n];
            for(int i = 0; i<answer.length; i++){
                if(i==0)   answer[i] = diff[1];
                else    answer[i] = answer[i-1] + diff[i+1];
            }
            return answer;
        }
    }
    ```

    