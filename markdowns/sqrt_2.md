# Square root, part 2 : Naive but efficient method

In the previous post, we implemented an efficient (or generally efficient) way to compute integer square root.

However, even if there are only a few steps, all the divisions, sums, ... take a lot of time while executed in BF. So, few iterations, but expensive ones...

A very naive approach would be to check 1, 2, 3, ... : is the current value still less than sqrt(n), or more precisely x^2 <= n ?

And this can be achieved very efficiently in BF, by subtracting odd numbers : example with 42
* test 0: 42 - 1 = 41 ( = 42 - 1^2), positive
* test 1: 41 - 3 = 38 ( = 42 - 2^2), positive
* test 2: 38 - 5 = 33 ( = 42 - 3^2), positive
* test 3: 33 - 7 = 26 ( = 42 - 4^2), positive
* test 4: 26 - 9 = 17 ( = 42 - 5^2), positive
* test 5: 17 - 11 = 6 ( = 42 - 6^2), positive
* test 6:  6 - 13 = -7 ( = 42 - 7^2), negative !

Result : 6

Due to the nature of BF language, it's far more efficient !!!

# Let's start

* Memory: N, 0, 0, 0, 0
* Cursor: on N 
* Input: any

# Process

* Memory: N, 0, 0, _current odd number_, _current result_
* Set current odd number to 1
* While N is not null
  * Decrease N
  * Decrease current odd number
  * If current odd is null (else flag previously set...)
    * increase current result
    * rebuild current odd number (2 x result + 1)
* Loop

# Code
```
>>>+<<<                   set result and odd
[                         while N is not null
  -                       decrease N
  >>+>-                   decrease odd (and set else flag before)
  [<-]<[-                 if odd number is null (else flag trick)
    >>+                   increase result
    [-<++<+>>]<+<[->>+<<] create new odd number
  <]
<]                        loop
>>>[-]                    cleansing
```

# Minified version
```
>>>+<<<[->>+>-[<-]<[->>+[-<++<+>>]<+<[->>+<<]<]<]>>>[-]
```

# Final state

* Memory: 0, 0, 0, 0, _iSqrt(n)_
* Cursor: just before result (4th cell)
* Input: unchanged
* Output: unchanged

# Test program

This program mixes all the previous steps : it reads an integer from the input and writes its iSqrt on the output
```
>,[>++++++[-<-------->]>+++++++++[<<<[->+>+<<]>>[-<<+>>]>-]<<[-<+>],]<   read integer
>>>+<<<[->>+>-[<-]<[->>+[-<++<+>>]<+<[->>+<<]<]<]>>>[-]>                 iSqrt
[>>>>++++++++++<<<<[->+>>+>-[<-]<[<<[->>>+<<<]>>>>+<<-<]<                print result
<]++++++++[->++++++<]>[-<+>]>>>>[-<<<<+>>>>]<[-]<<<]<[.<]                 ** part 2 **
```

We can see that this program is really shorter (and faster) than Newton's method !
