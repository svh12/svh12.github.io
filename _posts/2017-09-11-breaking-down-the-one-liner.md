---
layout: post
title: "Breaking Down The One-Liner"
date: 2017-09-11
---

I was trying to write a function that would return an array of prime numbers. My original code was verbose, so I searched for something more readable. I came across this one-liner and wanted to understand it better by breaking it down. I will be explaining the following one-liner:
<br>
<br>
let primeNumbers = &#91;Int&#93;(2...20).filter({num in &#91;Int&#93;(1...Int(sqrt(Float(num)))).filter({num % $0 == 0}).count == 1})
<br>
<br>
This one-liner uses the count and filter methods. The count method returns the number of elements in an array. The filter method returns a new array that contains only the elements that meet the specified condtions.
<br>
<br>
To break down the one-liner, I will explain each part of the code using traditional for-in loops and if statements. Each count or filter method represents a for-in loop, so prepare to see three for-in loops throughout this code breakdown.
<br>
<br>
// Import this library in order to use mathematical functions. We will be using sqrt() later.
<br>
import Foundation
<br>
<br>
// Set primeNumbers as an empty integer array. This will be our return array.
<br>
var primeNumbers: &#91;Int&#93; = &#91;&#93;
<br>
<br>
// Let's examine the first part of the one-liner: &#91;Int&#93;(2...20)
<br>
// These are the values we will loop through in our outer for-in loop. Set numberRange as an integer array containing a range of numbers from 2 to 20.
<br>
let numberRange = &#91;Int&#93;(2...20) // &#91;2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20&#93;
<br>
<br>
// Now let's look at the second part of the one-liner: num in &#91;Int&#93;(1...Int(sqrt(Float(num))))
<br>
// This shorthand closure refers to the outer for-in loop below, where "currentNum" is the current value in numberRange. It corresponds to "num" from the one-liner. For each currentNum in numberRange, we will want to do the enclosed.
<br>
for currentNum in numberRange {
<br>
<br>
	// Set sqrtArray as an integer array containing the range of numbers from 1 to the square root of currentNum.
  <br>
	// Because Doubles and Floats both represent floating point numbers and the sqrt() function takes a Double, we first need to convert currentNum from an Int to a Float: when currentNum = 10, Float(currentNum) = 10.0
  <br>
	// Now we can calculate the square root of the Float: sqrt(Float(currentNum)) = 3.16227766016838
  <br>
	// We then convert the Float back to an Int so that it conforms to the expected Int values of a closed range: Int(sqrt(Float(currentNum))) = 3
  <br>
	// This gives us the range of integers that will be the "dollarZero" values we loop through in our first inner for-in loop.
  <br>
	var sqrtArray = &#91;Int&#93;(1...Int(sqrt(Float(currentNum))))
  <br>
	<br>
	// Set remaindersArray as an empty integer array.
  <br>
	// This will be the array of remainders of currentNum.
  <br>
	// We will append each remainder to this array later.
  <br>
	var remaindersArray: &#91;Int&#93; = []
  <br>
	<br>
	// This is our first inner for-in loop, where "dollarZero" is the current value in sqrtArray. It corresponds to the "$0" from the one-liner.
  <br>
	// For each dollarZero in sqrtArray, we will want to do the enclosed.
  <br>
	for dollarZero in sqrtArray {
	<br>
  <br>
		// Set remainder as the remainder of num divided by dollarZero.
    <br>
		// For example: when num = 5 and dollarZero = 2, remainder = 1
    <br>
		var remainder = currentNum % dollarZero
    <br>
		<br>
		// Let's pause here and look back at the range of sqrtArray: (1...Int(sqrt(Float(currentNum))))
    <br>
		// Why did we want a range from 1 to the square root of currentNum?
    <br>
		// It's because the square root of currentNum will give us the highest divisor we need to test before determining whether or not currentNum is prime. The dollarZero values of sqrtArray show the smaller number of all the factor pairs.
    <br>
		// For example: the factor pairs of 12 are 1 * 12, 2 * 6, and 3 * 4. When currentNum = 12, sqrtArray = &#91;1, 2, 3&#93;.
    <br>
		// Thus, when we are calculating the value of remainder (currentNum % dollarZero), we do not need sqrtArray to cover a range from 1 to currentNum. Testing divisors beyond the square root of currentNum is unncessary.
    <br>
		<br>
		// After we calculate remainder, append the value of remainder into remaindersArray.
    <br>
		remaindersArray.append(remainder)
		<br>
    <br>
		// By the end of this first inner for-in loop, we will have an array of all the remainders for currentNum.
    <br>
		// For example: when currentNum = 10, remaindersArray = &#91;0, 0, 1&#93;
    <br>
	}	
	<br>
  <br>
	// Outside our first inner for-in loop, we will use a counter for how many elements in remaindersArray are equal to zero.
  <br>
	// Set remainderZeroCount equal to 0.
  <br>
	var remainderZeroCount = 0
  <br>
	<br>
	// Now we have arrived at our second inner for-in loop, where "remainderValue" is the current value in remaindersArray.
  <br>
	// For each remainderValue in remaindersArray, we will want to do the enclosed.
  <br>
	for remainderValue in remaindersArray {
  <br>
	<br>
		// This if statement will increment the remainderZeroCount if the remainderValue equals 0.
    <br>
		if remainderValue == 0 {
    <br>
			remainderZeroCount += 1
      <br>
		}
    <br>
		<br>
		// By the end of this second inner for-in loop, we will have a count of how many remainders are equal to 0 for currentNum.
    <br>
		// For example: when currentNum = 10, remainderZeroCount = 2.
    <br>
	}
	<br>
  <br>
	// Outside our second inner for-in loop, we will use an if statement that appends currentNum into the primeNumbers array if the remainderZeroCount of currentNum equals 1.
  <br>
	if remainderZeroCount == 1 {
  <br>
		primeNumbers.append(currentNum)
    <br>
	}
	<br>
  <br>
	// We only want to append the currentNum values that meet this condition because prime numbers are only divisible by 1 and itself.
  <br>
	// We eliminated the possibility of dividing a num value by itself when we set upper bound of sqrtArray equal to the square root of currentNum.
  <br>
	// This allows us to see if currentNum is evenly divisible only once by 1. If it is, then currentNum is a prime number.
  <br>
}
<br>
<br>
print(primeNumbers) // &#91;2, 3, 5, 7, 11, 13, 17, 19&#93;
<br>
<br>
The one-liner in this post generates an array of prime numbers within a specified range. To more easily change the range, you can also modify the code by declaring an upper bound variable:
<br>
<br>
// Set lowerBound and upperBound to desired numbers.
<br>
var lowerBound = 5
<br>
var upperBound = 20
<br>
<br>
// Change 2 to lowerBound and 20 to upperBound.
<br>
let primeNumbers = &#91;Int&#93;(lowerBound...upperBound).filter({num in &#91;Int&#93;(1...Int(sqrt(Float(num)))).filter({num % $0 == 0}).count == 1})
<br>
<br>
// To change the range, reset the values of lowerBound and upperBound as needed.
