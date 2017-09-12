---
layout: post
title: "Breaking Down The One-Liner"
date: 2017-09-11
---

I was trying to write a function that would return an array of prime numbers. My original code was verbose, so I searched for something more readable. I came across this one-liner and wanted to understand it better by breaking it down. I will be explaining the following one-liner:
<br>

<script src="https://gist.github.com/svh12/7bb3061ce7d62ab007a9aecf654ec6e5.js"></script>

<br>
This one-liner uses the ```count``` and ```filter``` methods. The ```count``` method returns the number of elements in an array. The ```filter``` method returns a new array that contains only the elements that meet the specified condtions. To break down the one-liner, I will explain the code using traditional for-in loops and if statements. Each ```count``` or ```filter``` method represents a for-in loop, so prepare to see three for-in loops throughout this code breakdown.
<br>

<script src="https://gist.github.com/svh12/fb551bf8ec83a828b4fe2c73d750c441.js"></script>

<br>
The one-liner in this post generates an array of prime numbers within a specified range. To more easily change the range, you can also modify the code by declaring an upper bound variable:
<br>

<script src="https://gist.github.com/svh12/3996dab57ae580be10ab3666abaf13b7.js"></script>

<br>

You can now reset the values of lowerBound and upperBound to change the range of prime numbers generated.
