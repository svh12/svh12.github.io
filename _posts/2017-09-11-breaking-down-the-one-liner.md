---
layout: post
title: "Breaking Down The One-Liner"
date: 2017-09-11
---

I was trying to write a function that would return an array of prime numbers. My original code was verbose, so I searched for something more readable. I came across this one-liner and wanted to understand it better by breaking it down. I will be explaining the following one-liner:
<br>

{% gist 7bb3061ce7d62ab007a9aecf654ec6e5 %}

<script src="https://gist.github.com/svh12/7bb3061ce7d62ab007a9aecf654ec6e5.js"></script>

<br>
This one-liner uses the ```count``` and ```filter``` methods. The count method returns the number of elements in an array. The filter method returns a new array that contains only the elements that meet the specified condtions. To break down the one-liner, I will explain the code using traditional for-in loops and if statements. Each count or filter method represents a for-in loop, so prepare to see three for-in loops throughout this code breakdown.
<br>

```swift
// Import this library in order to use mathematical functions. We will be using sqrt() later.
import Foundation

// Set primeNumbers as an empty integer array. This will be our return array.
var primeNumbers: [Int] = []

// Let's examine the first part of the one-liner: [Int](2...20)
// These are the values we will loop through in our outer for-in loop.
// Set numberRange as an integer array containing a range of numbers from 2 to 20.
let numberRange = [Int](2...20) // [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20]

// Now let's look at the second part of the one-liner: num in [Int](1...Int(sqrt(Float(num))))
// This shorthand closure refers to the outer for-in loop below, where "currentNum" is the current value in numberRange. It corresponds to "num" from the one-liner.
// For each currentNum in numberRange, we will want to do the enclosed.
for currentNum in numberRange {
	
	// Set sqrtArray as an integer array containing the range of numbers from 1 to the square root of currentNum.
	// Because Doubles and Floats both represent floating point numbers and the sqrt() function takes a Double, we first need to convert currentNum from an Int to a Float: when currentNum = 10, Float(currentNum) = 10.0
	// Now we can calculate the square root of the Float: sqrt(Float(currentNum)) = 3.16227766016838
	// We then convert the Float back to an Int so that it conforms to the expected Int values of a closed range: Int(sqrt(Float(currentNum))) = 3
	// This gives us the range of integers that will be the "dollarZero" values we loop through in our first inner for-in loop.
	var sqrtArray = [Int](1...Int(sqrt(Float(currentNum))))
	
	// Set remaindersArray as an empty integer array.
	// This will be the array of remainders of currentNum.
	// We will append each remainder to this array later.
	var remaindersArray: [Int] = []
	
	// This is our first inner for-in loop, where "dollarZero" is the current value in sqrtArray. It corresponds to the "$0" from the one-liner.
	// For each dollarZero in sqrtArray, we will want to do the enclosed.
	for dollarZero in sqrtArray {
		
		// Set remainder as the remainder of num divided by dollarZero.
		// For example: when num = 5 and dollarZero = 2, remainder = 1
		var remainder = currentNum % dollarZero
		
		// Let's pause here and look back at the range of sqrtArray: (1...Int(sqrt(Float(currentNum))))
		// Why did we want a range from 1 to the square root of currentNum?
		// It's because the square root of currentNum will give us the highest divisor we need to test before determining whether or not currentNum is prime. The dollarZero values of sqrtArray show the smaller number of all the factor pairs.
		// For example: the factor pairs of 12 are 1 * 12, 2 * 6, and 3 * 4. When currentNum = 12, sqrtArray = [1, 2, 3].
		// Thus, when we are calculating the value of remainder (currentNum % dollarZero), we do not need sqrtArray to cover a range from 1 to currentNum. Testing divisors beyond the square root of currentNum is unncessary.
		
		// After we calculate remainder, append the value of remainder into remaindersArray.
		remaindersArray.append(remainder)
		
		// By the end of this first inner for-in loop, we will have an array of all the remainders for currentNum.
		// For example: when currentNum = 10, remaindersArray = [0, 0, 1]
	}	
	
	// Outside our first inner for-in loop, we will use a counter for how many elements in remaindersArray are equal to zero.
	// Set remainderZeroCount equal to 0.
	var remainderZeroCount = 0
	
	// Now we have arrived at our second inner for-in loop, where "remainderValue" is the current value in remaindersArray.
	// For each remainderValue in remaindersArray, we will want to do the enclosed.
	for remainderValue in remaindersArray {
		
		// This if statement will increment the remainderZeroCount if the remainderValue equals 0.
		if remainderValue == 0 {
			remainderZeroCount += 1
		}
		
		// By the end of this second inner for-in loop, we will have a count of how many remainders are equal to 0 for currentNum.
		// For example: when currentNum = 10, remainderZeroCount = 2.
	}
	
	// Outside our second inner for-in loop, we will use an if statement that appends currentNum into the primeNumbers array if the remainderZeroCount of currentNum equals 1.
	if remainderZeroCount == 1 {
		primeNumbers.append(currentNum)
	}
	
	// We only want to append the currentNum values that meet this condition because prime numbers are only divisible by 1 and itself.
	// We eliminated the possibility of dividing a num value by itself when we set upper bound of sqrtArray equal to the square root of currentNum.
	// This allows us to see if currentNum is evenly divisible only once by 1. If it is, then currentNum is a prime number.
}

print(primeNumbers) // [2, 3, 5, 7, 11, 13, 17, 19]
```

<br>
The one-liner in this post generates an array of prime numbers within a specified range. To more easily change the range, you can also modify the code by declaring an upper bound variable:
<br>

```swift
// Set lowerBound and upperBound to desired numbers.
var lowerBound = 5
var upperBound = 20

// Change 2 to lowerBound and 20 to upperBound.
let primeNumbers = [Int](lowerBound...upperBound).filter({num in [Int](1...Int(sqrt(Float(num)))).filter({num % $0 == 0}).count == 1})
```

You can now reset the values of lowerBound and upperBound to change the range of prime numbers generated.
