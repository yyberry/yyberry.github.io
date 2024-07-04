---
layout: post
title: Binary Search
subtitle: This is the learning notes for Binary Search 
# cover-img: /assets/img/cover1.png
# thumbnail-img: /assets/img/thumbnail1.jpeg
# share-img: /assets/img/cover1.png
tags: [Algorithms]
---

This is a learning note related to the binary search algorithm. It contains the basic ideas of the binary search and some practice examples. 

## Bianry Search

Binary Search is a basic algorithm used to search specific value in an ordered array. The basic idea of binary search is to divide n elements into two roughly equal parts and compare `a[n/2]` with `x`. if `x = a[n/2]`, x is found and the algorithm is aborted. If `x<a[n/2]`, then continue searching for `x` in the left half of array `a`, if `x>a[n/2]`, then just search for `x` in the right half of array `a`. The time complexity is the number of loops.

### Practice

1. Define a auxiliary function $f$ that presents the relationship between $x$ with $f(x)$, and given a $target$ where $target = f(x_i)$. **$x$ is what the the question asked for.**
2. according the question to decide whether the left or right boundary is required. Using binary search to get the required boundary.

* **1011** Capacity To Ship Packages Within D Days

  * **Question:** A conveyor belt has packages that must be shipped from one port to another within `days` days.

    The $i^{th}$ package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.

    Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.

  * **Solution:** In this case, $x$ is the maximum capacity of the ship, $f(x)$ is the number of delivery days. The $target$ is given `days` . The relationship between them is as following:

    <div align = center><img src="/assets/img/bs_f(x).png" style="zoom:30%;" /></div><center> auxiliary function where `target` is D</center>

    Here we need to reach the left boundary. Left boundary is the maximum weights amoung the packages, while the right boundary is one more than the sum weight of all packages. **Since the ship should convey at least one package at a time, and at most all package at a time.** And the **package is indivisible**, which means, if there is still capacity left on the ship, but the remaining capacity is less than the package weight, the ship cannot convey a part of the package.

  * **Code:**

    * Java:

      ```java
      class Solution {
          public int shipWithinDays(int[] weights, int days) {
              // set the left and right boundary for capacity, left closed right open
              int left = 0;
              int right = 1;
              for(int w : weights){
                  left = Math.max(left, w);
                  right += w;
              }
              
              // use binary search to get the left boundary
              while(left < right){
                  int mid = left + (right - left) / 2;
                  if(f(weights, mid) <= days){
                      right = mid;
                  }
                  else{
                      left = mid + 1;
                  }
              }
      
              return left;
          }
      
          // auxiliary function
          // compute the number of days with given capacity
          public int f(int[] weights, int cap){
              int days = 0;
              for(int i = 0; i < weights.length; ){
                  int x = cap;
                  // traverse the list 
                	// until the packages exceed the maximum weight capacity of the ship
                  while(i < weights.length){
                      if(x < weights[i]) break;
                      else x -= weights[i];
                      i ++;
                  }
                  days ++;
              }
              return days;
          }
      }
      ```

    * Python:

      ```python
      class Solution(object):
          def shipWithinDays(self, weights, days):
              """
              :type weights: List[int]
              :type days: int
              :rtype: int
              """
              # x: capacity of the ship
              # f(x): days of shipping
              # target: days
              # find x_1 to make f(x_1)=target         
              
              left = max(weights) # left boundary of x
              right = sum(weights) # right boundary of x
      
              while left < right:
                  mid = left + (right - left) // 2
                  if self.f(weights, mid) <= days: # dont forget self !!!!
                      right = mid
                  else:
                      left = mid + 1
              
              return left
      
          def f(self, weights, x):
              i, l = 0, len(weights)
              days = 0
              while i < l:
                  capacity = x
                  while i < l: # loops condition
                      if weights[i] <= capacity:
                          capacity -= weights[i]
                          i += 1 # no ++ in python
                      else:
                          break
                  days += 1
              return days
      ```

  

* **875** Koko Eating Bananas

  * Questions: Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

    Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

    Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

    Return *the minimum integer* `k` *such that she can eat all the bananas within* `h` *hours*.

  * Solution: 

    * same as last question, the point is to find which value refers to $x$, $f(x)$ and $target$. 
    * In this case $x$ is `k`, $f(x)$ is the number of hours, $target$ is `h`. 
    * Be careful the left and right boundary refer to the min and max number of bananas Koko eats per hour. **Koko doesn't need to eat at least one pile per hour, so the left boundary is not the number of bananas in the maximum pile**. 
    * One pile may take her several hours to finish eating. But **if Koko eat up one pile, she will not eat any other bananas in another pile**.

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

    

* **704** Bianry Search

  * Question: Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

    You must write an algorithm with `O(log n)` runtime complexity.

  * Solution: super simple example of using binary search directly. Be careful if the target cannot be found, we should return -1.

  * Code:

    * Java：

      ```java
      class Solution {
          public int search(int[] nums, int target) {
              int left = 0, right = nums.length;
              while(left < right){
                  int mid = left + (right - left) / 2;
                  if(nums[mid] == target) return mid;
                  else if(nums[mid] > target) right = mid;
                  else left = mid + 1;
              }
              return -1;
          }
      }
      ```

    * Python:

       ```python
       class Solution(object):
           def search(self, nums, target):
               """
               :type nums: List[int]
               :type target: int
               :rtype: int
               """
               left, right = 0, len(nums) # assignment statement
               while left<right: #[...)
                   mid = left + (right - left) // 2 #to prevent int overflow
                   if nums[mid]==target:
                       return mid
                   elif nums[mid]>target:
                       right = mid # right open
                   else:
                       left = mid + 1 # left closed
       
               return -1
       
       ```

    

   * **74** Search a 2D Matrix

     * Question: You are given an `m x n` integer matrix `matrix` with the following two properties:

       - Each row is sorted in non-decreasing order.
       - The first integer of each row is greater than the last integer of the previous row.

       Given an integer `target`, return `true` *if* `target` *is in* `matrix` *or* `false` *otherwise*.

       You must write a solution in `O(log(m * n))` time complexity.

     * Solution: Same as previous one, but here we need to flatten the 2D matrix as 1D list in our mind.

     * Code:

       ```java
       class Solution {
           public boolean searchMatrix(int[][] matrix, int target) {
               int m = matrix.length;
               int n = matrix[0].length;
               int left = 0, right = m*n-1;
               while(left<=right){
                   int mid = (right-left)/2 + left;
                   if(matrix[mid/n][mid%n] == target) return true;
                   else if(matrix[mid/n][mid%n] > target) right = mid-1;
                   else left = mid + 1;
               }
               return false;
           }
       }
       ```

       

       

        