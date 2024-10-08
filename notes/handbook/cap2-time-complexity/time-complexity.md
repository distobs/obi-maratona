# Time complexity

## Definition

The time complexity estimates how much time the algorithm uses for some input
size.

## Rules

### Loops

If there's a loop through the input, the complexity is O(n).

If there are k nested loops through the input, the complexity is O(n\^k).

### Order of magnitude

The time complexity only shows us the order of magnitude, but not how many
times the code inside a loop will be executed.

### Phases

If an algorithm has multiple phases, the phase with the highest cost defines
the complexity.

### Variables

When the time complexity depends on several values, it contains several
variables.

```c
/* O(mn) */

for (int i = 0; i < n; ++i) {
	for (int j = 0; j < m; ++j) {
		/* stuff */
	}
}

```

### Recursion

The time complexity of a recursive function depends on how many times we call
it.

```c
void
f(int n)
{
	if (n == 1) {
		return;
	}

	f(n - 1);
}
```

Calling f(n) would cause n function calls. With the time complexity of each
call being O(1), all calls made would amount to O(n).


```c
void
g(int n)
{
	if (n == 1) return;

	g(n - 1);
	g(n - 1);
}
```

|   call    | number of calls |
| --------- | --------------- |
| 1. g(n)   | 1               |
| 2. g(n-1) | 2               |
| 3. g(n-2) | 4               |

The m-th function is called 2\^(m-1) times. Thus, the n-th function, g(1), would
be called 2\^(n-1) times.

Thus, our complexity function is defined by the sum of the terms of a geometric
progression where $a_1 = 1 \land k = 2 \land b = 2^{n-1}$.

We can find the sum using the formula:

$$S = \frac{bk - a}{k - 1} \iff$$

$$\implies S = \frac{2^{n-1} \cdot 2 - 1}{2 - 1} = 2^n - 1$$

Throwing that function inside the big-O, we now know our time complexity is
O(2^n).

## Complexity Cases

- O(1): Constant-time, independent of input size.

- O(log n): Logarithmic, often halving the input size at each step.

- O(sqrt(n)): Square root algorithm, faster than O(n) but slower than O(log n).

- O(n): Linear, often the best possible complexity.

- O(nlog n): Indicates that the input was sorted, since the best sorting
  algorithms are O(nlog n). Can indicate that the algorithm uses some data
  structure where each operation takes O(log n).

- O(n\^2): Quadratic. Contains two nested loops. Goes through pairs.

- O(n\^3): Cubic. Contains three nested loops. Goes through triplets.

- O(2\^n): Indicates the algorithm iterates through all the subsets of the
input.

- O(n!): Factorial. Indicates the algorithm iterates through all permutations
  of the input.

An algorithm is polynomial if its time complexity is no more than O(n\^k),
where k is a constant. It roughly means the algorithm is efficient.

## Estimating efficiency

The starting point for guessing an algorithm's efficiency is knowing that
modern computers perform > 100 000 000 instructions in one second.

For example, if n = 10\^5 and our algorithm is O(n\^2), the algorithm will
roughly perform (10\^5)\^2 = 10\^10 operations, which will take some 10 seconds
or more.

Given the input size, we can guess what complexity problem wants from us. The
following table has some useful guesses.

|     Size    |    Complexity     |
| ----------- | ----------------- |
| n <= 10     | O(n!)             |
| n <= 20     | O(2\^n)           |
| n <= 500    | O(n\^3)           |
| n <= 5000   | O(n\^2)           |
| n <= 10\^6  | O(n log n) / O(n) |
| n > 10\^6   | O(1) / O(log n)   |

## Maximum subarray sum

Given an array of n numbers, calculate the largest possible sum of a sequence
of consecutive values.

### First solution

The naïve way is going through all possible subarrays, calculating the sum
while keeping the best sum in a variable.

```c
int best = 0;

for (int i = 0; i < n; ++i) {
	for (int j = i; j < n; ++j) {
		int sum = 0;
		for (int k = i; k <= j; ++k) {
			sum += array[k];
		}
	}

	best = max(best, sum);
}
```

This solution is O(n\^3), because it has three nested loops going through the
input.

### Second solution

```c
int best = 0;

for (int i = 0; i < n; ++i) {
	int sum = 0;
	for (int j = i; j < n; ++j) {
		sum += array[j];
		best = max(best, sum);
	}
}
```

This optimized solution is O(n\^2), because it takes one loop away from the
three.

### Third solution

```c
int best = 0, sum = 0;

for (int i = 0; i < n; ++i) {
	sum = max(array[i], sum + array[i]);
	best = max(best, sum);
}
```

The algorithm calculates, for each array position, the max sum of a subarray
ending at that position.

There are two possibilities for the subproblem of finding the max-sum subarray
ending at k:

1. The subarray contains only array\[k\], that is, array\[k\] starts a possible
subarray.

2. The subarray consists of a subarray ending at k - 1, with the addition of
array\[k\], that is, array\[k\] is either the last element in a possible
subarray, or it's in the middle of one.
