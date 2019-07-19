# Print a number

Now, with euclidean division, we can print number. The process looks like (recursive definition): print(x) = if x < 10 then return x, else return print(quotient x/10) + remainder x/10

In other words, we will divide N successively by 10, store the remainders and fianlly print them all, backards.

Note : the remainders will first be values from 0 to 9. But
* it's uneasy to iterate on null values (see playground about arrays)
* and we need to print chars from "0" to "9" and not just values

Fortunately, "0" is not 0, so we will store remainders + 48

# Let's start

* Memory: 0, N, 0, ... (note : we will need a various number of zeros, based on how many digits are displayed)
* Cursor: on N
* Input: any

# Process

* _Invariant : memory = 0, {some digit chars}, X, 0, 0, 0, 0, 0 with N in base 10 written as "X in base 10" + all the digit chars, reversed_
* While X is not null
  * Divide X by 10
  * Add 48 to remainder
  * Replace X cell by remainder
  * Put quotient after X cell
  * Clear all used cells on the right
  * Go to the "new X cell" (on the right of the original)
* Loop
* _Memory = 0, {N number in base 10, with reversed digits}, 0, 0, 0, 0, 0_
* Print all chars in array, backwards

# Code
```
[                        While X is not null
  >>>>++++++++++<<<<     (10)
  [->+>>+>-[<-]<[<<[->>  Divide X by 10
  >+<<<]>>>>+<<-<]<<]    ** part 2 **
  ++++++++[->++++++<]>   add 48 to remainder
  [-<+>]                 Move remainder at X location
  >>>>[-<<<<+>>>>]       Put quotient at new X location
  <[-]<<<                Reset cells
]                        Loop
<[.<]                    Print backwards
```

#Minified version
```
[>>>>++++++++++<<<<[->+>>+>-[<-]<[<<[->>>+<<<]>>>>+<<-<]<<]++++++++[->++++++<]>[-<+>]>>>>[-<<<<+>>>>]<[-]<<<]<[.<]
```

# Final state

* Memory: 0, { reversed digits of N}, 0
* Cursor: on first cell
* Input: unchanged
* Output: N printed, in base 10

# Test program

This program reads all chars from the input and print ASCII codes (space separated)

```
>,[                         for each char read
  [>>>>++++++++++<<<<[->+   Print char code
  >>+>-[<-]<[<<[->>>+<<<]    ** part 2 **
  >>>>+<<-<]<<]++++++++[-    ** part 3 ** 
  >++++++<]>[-<+>]>>>>[-<    ** part 4 ** 
  <<<+>>>>]<[-]<<<]<[.[-]<]  ** part 5 ** Note: the "reset cell" before end of line : chars are deleted after being printed
  ++++++++[->++++<]>.       (32) = space; and print
,]                          Read next char and loop
```
