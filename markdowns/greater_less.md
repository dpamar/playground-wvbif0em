# Evaluate greater / less functions

We already know how to evaluate whether 2 numbers are equals.
Given 2 numbers A and B, let's see now how to check if A is less than to B (LT) or greater or equal to B (GE).

This code can be obviously adapted from GE / LT to LE / GT

# Let's start

* Memory: A, B, O, O
* Cursor: on A
* Input: any

# Process

* _Invariant : first cell - second cell is always d = A - B. If B ends to 0, then d > 0; if A ends to 0 first then d <= 0_
* While A is not null
  * Set else flag on 3rd cell
  * Decrease A
  * If B is not null
    * Decrease B
    * Reset else flag
  * If B is null
    * A is greater than B, no need to continue the loop : reset A
* Loop
* Result is on B: "B not null" == "A < B"

# Code
```
[             while A is not null
  -           decrease A
  >>+         set else flag
  <[          if B is not null
    ->-       decrease B and reset else flag
  ]>          B not null : on 4th cell; B null : on 3rd cell set to 1
  [<<[-]>>->] so if B null reset A
  <<<         back to A
]             loop
>[[-]<+>]<    a bit formatting (if result is true then clear B and put 1 into A)
```

# Minified version
```
[->>+<[->-]>[<<[-]>>->]<<<]>[[-]<+>]<
```

# Final state

* Memory: _result_, 0, 0, 0 with result = 1 if B > A and 0 if A >= B
* Cursor: on result
* Input: unchanged
* Output: unchanged

# Test program

This program reads 2 chars. It first copies these chars, then compares to print in alphabetical order

```
,[->+>+<<]>[-<+>],[->>+>+<<<]>>>[-<<<+>>>]<<   read 2 chars and copy
[->>+<[->-]>[<<[-]>>->]<<<]>[[-]<+>]<          compare the 2 copies
>+<[>-<-<<.>.>]                                set else bit; if A less than B print A then B (and reset result)
>[-<<.<.>>>]                                   otherwise print B then A
```
