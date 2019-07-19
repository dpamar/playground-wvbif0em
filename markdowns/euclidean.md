# Euclidean division

Now we are able to read an interger. The next step would be ability to *print* an integer.

However, we first need to have an algorithm for euclidean division in order to implemented the "print integer" program.

Euclidean division is an operation on A (dividend) and B (divisor) that returns Q (quotient) and R (remainder), with
* A=B * Q + R
* 0 <= R < B

In BrainFuck, this can be implemented like this
* Decrease dividend
* Increase remainder
* Decrease divisor
* If divisor = 0 then
  * note that now, remainder = initial divisor
  * so: rebuild initial divisor from remainder
  * reset remainder
  * increase quotient

and repeat as long as dividend is not null. When it reaches 0, then we have the remainder, the quotient,....

# Let's start

* Memory: A, 0, 0, 0, B, 0
* Cursor: on A
* Input: any

# Process

* _Invariant : memory = dividend, remainder, 0, else flag, divisor, quotient with B = divisor + remainder and A = quotient * B + dividend + remainder_
* while dividend is not null
  * decrease dividend
  * increase remainder
  * set else flag
  * decrease divisor
  * _invariant is still valid here_
  * If divisor is not null reset else flag
  * Go left
    * divisor not null : between remainder and else flag (0)
    * divisor null : on else flag (1)
  * if not null (divisor null, on else flag)
    * rebuild divisor (move remainder to divisor)
    * increment quotient
    * reset else bit
    * move left
  * _again invariant is still valid here_
  * In all cases we are between remainder and else flag
* Loop
* _Memory = 0, remainder, 0, 0, divisor, quotient; with A = quotient * B + remainder_

# Code
```
[                    while dividend is not null
  -                  decrease dividend
  >+                 increase remainder
  >>+                set else flag
  >-                 decrease divisor
  [<-]               if divisor is not null reset else flag
  <[                 and if divisor is null
    <<[->>>+<<<]     rebuild divisor
    >>>>+            increase quotient
  <<-<]              move between remainder and else flag (reset it btw)
<<]                  loop
```

# Minified version
```
[->+>>+>-[<-]<[<<[->>>+<<<]>>>>+<<-<]<<]
```

# Final state

* Memory: 0, R, 0, 0, B', Q with A = B * Q + R and B = B'+R
* Cursor: on first cell
* Input: unchanged
* Output: unchanged

#Test program

This code divides 131 by 2 and print quotient (65, ASCII code A) as many times as the remainder
```
>++++++++++[-<+++++++++++++>]<+          (131)
>>>>++<<<<                               (2)
[->+>>+>-[<-]<[<<[->>>+<<<]>>>>+<<-<]<<] division
>[>>>>.<<<<-]                            and print quotient as many times as the remainder
```

Result : A

You can replace first two lines by these examples
* +++++[->+++++<]->[-<++++++>]>>>+++<<<< : 149 = 49 x 3 + 2, it prints "1" 2 times
* ++++++++[->++++<]+++>[-<+++++>]>>>++++<<<< : 163 = 40 x 4 + 3, it prints "(" 3 times
* etc...


