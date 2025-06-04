Let's count how many digits a number on base 10 has. In order to do this, we can use two approaches:
- Loop dividing by 10
- Log base 10.
## Loop dividing by 10

When we perform an integer division from the number by 10, we're actually extracting the rightmost digit from a number. For example, `121 / 10` will result in `12` and the remainder will be 1. Now, if we perform `12 / 10`, it will result in `1` and the remainder will be 2. Finally, if we perform,  `1 / 10`, it'll result in 0 and the remainder will be `1`. 

We can use the result of this operation to loop over the entire number while it's not 0. Like this:
```c++
int countDigits(int num) {
	int qtt = 0;
	while(num > 0) {
		num /= 10;
		qtt++;
	}
	return qtt;
}
```

If the number is negative, we can get the absolute value of it, because it follows the same logic.
### Why it works?

It works because our numeric positional system have as basis the number 10, so the integer division by 10 will extract the rightmost number one by one until 0.

## Log base 10

The process above can be summarized to: `num / 10^x`. We need this operation to result in 1, because this will guarantee that we have counted all the numbers inside the number. So, we can represent this like the following operation: `10^x = num`. 

In the real world, we know the number of `num`, so, we can say the following: `10^x = 121`. We can use log to discover the value of x: `log10 x = 121`. Since this operation will result in an integer number only for `10^1, 10^2, 10^3...`, we will get a float number. This float number will be closer to the number of 0s in the number.

For example:
- 121 will be something like `10^2.12..`,
- 1212 will be something like `10^3.1231`

So, the log base 10 will count the number of digits before the leftmost number. Then, we can do the following:

```c++
int countDigits(int num) {
	return floor(log10(abs(num))) + 1;
}
```
