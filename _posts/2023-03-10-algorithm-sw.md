---
layout: post
title: Sliding Window
subtitle: This is the learning notes for Sliding Window
# cover-img: /assets/img/cover1.png
# thumbnail-img: /assets/img/thumbnail1.jpeg
# share-img: /assets/img/cover1.png
tags: [Algorithms]
---

This is a learning note related to the sliding window algorithm. It contains the basic ideas of the sliding window and some practice examples.

## Sliding Window

Sliding window protocol (Sliding Window Protocol), which is an application of TCP protocol, is used for flow control during network data transmission to avoid congestion. The protocol allows a sender to send multiple data packets before stopping and waiting for an acknowledgement. **Since the sender does not have to stop and wait for an acknowledgement every time a packet is sent. **Therefore, the protocol can accelerate the data transmission and improve the network throughput.

The sliding window algorithm is actually the same as this protocol, but the implementation scene is different. **The sliding window algorithm operates on a string or array with a specific size,** rather than on the entire string and array, thus it can reduce the complexity of the problem and the nesting depth of the loop. **Sliding windows are mainly used in arrays and strings.**

### Practice

1. initialize left and right pointers to limit size of sliding window. 

2. move the right and left pointers to make the data in window fullfills the requirements.

   

* 3 Longest Substring Without Repeating Characters

  * Question: Given a string `s`, find the length of the **longest** **substring** without repeating characters.

  * Solution: first move right pointer until there are repeating characters in the window, then move left pointer to shrink window until no repeating characters in window. During this process, update the result. 

  * Code:

    ```java
    class Solution {
        public int lengthOfLongestSubstring(String s) {
            // initialize left and right pointer at the beginning of string
            int left = 0, right = 0; 
    
            // rkeep tract of the result
            int res = 0;
    
            // sliding window, contains the charcters between left and right
            HashMap<Character, Integer> window = new HashMap<Character, Integer>();
            
            // forward the right pointer
            while(right < s.length()){
                char r = s.charAt(right);
                right ++;
    
                // while there are repeating chacters in the window, shrink window
                while(window.containsKey(r)){
                    char l = s.charAt(left);
                    left ++;
                    window.remove(l);
                }
    
                // aad new (key, value) pair in the window
                window.put(r, right);
    
                //update the result
                res = Math.max(res, right-left);
            }
    
            return res;
        }
    }
    ```

* 76 Minimum Window Substring

  * Question: Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the **minimum window*** 

    **substring** *of* `s` *such that every character in* `t` *(**including duplicates**) is included in the window*. If there is no such substring, return *the empty string* `""`.

    The testcases will be generated such that the answer is **unique**.

  * Solution: Same as previous, but here a fixed size list is used to keep track of the number of characters in sliding window. And its initialization is defined by the `t`. 

  * Code: 

    ```java
    class Solution {
        public String minWindow(String s, String t) {
            // use a list to store the number of each char in s
            // initialize the list with the number of each char in t
            int[] map = new int[128];
            for (char c : t.toCharArray()) {
                map[c] += 1;
            }
            // keep track of the required substring
            int begin = 0;
            int len = Integer.MAX_VALUE;
            // record the number of the needed character
            int count = t.length();
            // enlarge the window
            for (int left=0, right=0; right<s.length(); right++) {
                char c = s.charAt(right);
                map[c]--;
                // if needed char is in the window, count --
                if(map[c]>=0) count--; 
                // while all needed chars are in the window, shrink the window
                while (count == 0) {
                    char lc = s.charAt(left);
                    map[lc]++;
                    // when shrinking the window
                    // not all required chars are in the window
                    // update the minimum window
                    if (map[lc]>0) {
                        if (right-left+1<len) {
                            begin = left;
                            len = right-left+1;
                        }
                        count++;
                    }
                    left++;
                }
            }
            return len==Integer.MAX_VALUE?"":s.substring(begin, begin+len);
        }
    }
    ```

    