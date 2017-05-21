---
title: Edit Distance Problem
date: 2017-05-20 22:34:18
tags:
  - Algorithms
  - English
---

"Edit Distance" is the number of steps required to convert from word A to word B. I was asked to find minimum steps in a past on-site interview as a second coding question. I've never heard about the concept of "Edit Distance" before that interview, and I wasn't able to find a good solution to that problem at that time neither.

After the interview, my wife told me that she saw this problem in [LeetCode](https://leetcode.com/problems/edit-distance/), then I found it and finally solved it.

<!-- more -->

One thing I'd like to point out firstly is that this is a "Hard" problem in LeetCode, as an interviewer, I'll probably never use this problem to test my interviewees. Because this problem doesn't have a smooth solving curve, it would only be either a brute-force solution or a highly optimized one, which means there isn't a solution in the middle of t. Even candidates come up with the right algorithm, they still need to keep optimizing their thought otherwise they may still not be able to solve the problem.

The original problem could be found here: [LeetCode](https://leetcode.com/problems/edit-distance/).

### Description
Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:
* Insert a character
* Delete a character
* Replace a character

### Analysis

### Solution
{% codeblock lang:java %}
public int minDistance(String word1, String word2) {

    // Handle corner cases

    if ((word1 == null || word1.isEmpty()) &&
        (word2 == null || word2.isEmpty())) {
        return 0;
    }

    if (word1 == null || word1.isEmpty()) {
        return word2.length();
    }

    if (word2 == null || word2.isEmpty()) {
        return word1.length();
    }

    // build a matrix to track states.
    final int wordOneLength = word1.length();
    final int wordTwoLength = word2.length();

    int[][] dp = new int[wordTwoLength][wordOneLength];

    char charOne = word1.charAt(0);
    char charTwo = word2.charAt(0);

    if (charOne == charTwo) {
        dp[0][0] = 0;
    } else {
        dp[0][0] = 1;
    }

    // initialize first row and col
    boolean isCharFound = false;  // Track if a char has already appeared before.

    for (int wordOneIndex = 1; wordOneIndex < wordOneLength; wordOneIndex++) {
        if (!isCharFound && charTwo == word1.charAt(wordOneIndex)) {
            dp[0][wordOneIndex] = dp[0][wordOneIndex - 1];
            isCharFound = true;
        } else {
            dp[0][wordOneIndex] = dp[0][wordOneIndex - 1] + 1;
        }
    }

    isCharFound = false;

    for (int wordTwoIndex = 1; wordTwoIndex < wordTwoLength; wordTwoIndex++) {
        if (!isCharFound && charOne == word2.charAt(wordTwoIndex)) {
            dp[wordTwoIndex][0] = dp[wordTwoIndex - 1][0];
            isCharFound = true;
        } else {
            dp[wordTwoIndex][0] = dp[wordTwoIndex - 1][0] + 1;
        }
    }

    for (int wordTwoIndex = 1; wordTwoIndex < wordTwoLength; wordTwoIndex++) {
        charTwo = word2.charAt(wordTwoIndex);

        for (int wordOneIndex = 1; wordOneIndex < wordOneLength; wordOneIndex++) {

            charOne = word1.charAt(wordOneIndex);

            if (charOne == charTwo) {
                dp[wordTwoIndex][wordOneIndex] = dp[wordTwoIndex - 1][wordOneIndex - 1];
            } else {
                int min = Math.min(dp[wordTwoIndex - 1][wordOneIndex - 1], dp[wordTwoIndex][wordOneIndex - 1]);
                min = Math.min(min, dp[wordTwoIndex - 1][wordOneIndex]);
                dp[wordTwoIndex][wordOneIndex] = min + 1;
            }
        }
    }

    return dp[wordTwoLength - 1][wordOneLength - 1];
}
{% endcodeblock %}
