# Welcome!

Welcome to this new playground about the BF language. Make sure you already read 
* [Getting started with BrainFuck](https://tech.io/playgrounds/50426/getting-started-with-brainfuck/welcome)
* [BrainFuck part 2 - Working with arrays](https://tech.io/playgrounds/50443/brainfuck-part-2---working-with-arrays/welcome)
* [BrainFuck part 3 - Write a BF interpreter in BF](https://www.codingame.com/playgrounds/50446/brainfuck-part-3---write-a-bf-interpreter-in-bf/welcome)

playgrounds if you didn't already !

The goal of this playground is to play with maths a bit more. Let's start with something simple : read a number.

In all the previous playground steps, data were read as "chars", mapped to ASCII values, but what if we want to type 123 and have a value 123 (instead of 3 chars)?

# Let's start

* Memory: 0, 0, 0, 0
* Cursor: on first cell
* Input: a decimal number

# Process

* _invariant : first cell is the actually read number_
* Read next digit on second cell
* While a digit has been read
  * Convert char into digit (-48)
  * Multiply number of first cell by ten 
  * Add second cell to first
    * More exactly : add first cell 9 times to the second (needs less extra memory)
    * then add second to first
* loop
* _first cell : the decimal number_

# Code
```
>,                      read next digit on second cell
[                       while digit is read
  >++++++[-<-------->]  convert into digit
  >+++++++++[           do 9 times
    <<<[->+>+<<]        add first cell to second and create a save copy
    >>[-<<+>>]          put save copy back on first cell
  >-]                   loop (9 times)
  <<[-<+>],             add second to first then read next digit
]<                      loop
```

# Minified version
```
>,[>++++++[-<-------->]>+++++++++[<<<[->+>+<<]>>[-<<+>>]>-]<<[-<+>],]<
```

# Final state

* Memory: N, 0, 0, 0
* Cursor: on N
* Input: empty 
* Output: unchanged

# Test program
This very basic program reads ASCII code and prints it

```
>,[>++++++[-<-------->]>+++++++++[<  read number
<<[->+>+<<]>>[-<<+>>]>-]<<[-<+>],]<  ** part 2 **
.                                    print as ASCII code
```

Try with inputs from 65 to 90 for example to have all uppercased letters

