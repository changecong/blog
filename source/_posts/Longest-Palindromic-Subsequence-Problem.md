---
title: Longest Palindromic Subsequence Problem
date: 2017-05-21 14:08:42
tags:
  - Algorithms
  - English
  - Dynamic Programming
thumbnailImage: algorithm.jpg
thumbnailImagePosition: left
---

This is another problem from [LeetCode](https://leetcode.com/problems/longest-palindromic-subsequence/).

<!-- excerpt -->

### Description
Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

### Analysis
The brute-force solution of this problem is to find all the subsequence of the string and determine if they are palindromic or not. Then find the length of the longest palindromic one.

The the time complexity would be as same as all the combination of chars of the string, which is O(2^n). And this complexity is usually unacceptable.

While working on this type of problem, the very first optimization would be using dynamic programming (DP). Then let's consider how to find the longest palindromic by using previous states.

* If a string is palindromic, then its first character and the last one should be same.
* Once the first and last character is same, the length of the longest palindromic substring should be the longest of the string excludes the first and last character plus 2.

Let's use a 2-d matrix longest[][] whose elements longest[row][col] represent the length of the longest palindromic substring (character at row & col are included), then we have the formula to calculate it using DP:
{% codeblock %}
if char at row equals to char at col:
  longest[row][col] = longest[row + 1][col - 1] + 2
else
  longest[row][col] = max between longest[row + 1][col] and longest[row][col - 1]
{% endcodeblock %}

### Solution
{% codeblock lang:java %}
public int longestPalindromeSubseq(String s) {
    if (s == null) {
        return 0;
    }

    final int length = s.length();

    final int[][] dp = new int[length][length];

    // dp[i][j]: longest from char i to char j
    //
    // if char(i) == char(j)
    //   dp[i][j] = dp[i + 1][j - 1] + 2
    // else
    //   max(dp[i + 1][j], dp[i][j - 1])

    for (int row = length - 1; row >= 0; --row) {

        // diagonal
        dp[row][row] = 1;

        // only need to consider half of the matrix
        for (int col = row + 1; col < length; ++col) {
            if (s.charAt(row) == s.charAt(col)) {
                dp[row][col] = dp[row + 1][col - 1] + 2;
            } else {
                dp[row][col] = Math.max(dp[row + 1][col], dp[row][col - 1]);
            }
        }
    }

    return dp[0][length - 1];
}
{% endcodeblock %}
