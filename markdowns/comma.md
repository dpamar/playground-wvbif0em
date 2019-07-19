# Read comma separated numbers

We know how to read one number. Let's do something more complex : read multiple numbers, with comma separator.

Process is rather simple:
* Read a char
  * It's a digit : part of the current number
  * It's a comma : start a new number

# Let's start

* Memory: _empty_
* Cursor: first cell
* Input: A, B, C, ... (comma separated integer values)

# Process

* Leave first cell empy (array delimiter)
* Start a number (leave another empty cell)
* While a char is read
  * Subtract 44 ("," ASCII code) and set an else flag
  * Result is not null
    * Subtract again 4 (44+4 = 48, and -48 converts a digit char into a digit value)
    * Update current integer (add it nine times to the actual digit, then this whole block to the current integer)
    * Reset else flag
  * Result is not null
    * Start a number (leave another empty cell)
* Loop

# Code
```
>>                        Leave first cell empy (array delimiter); Start a number (leave another empty cell)
,[                        While a char is read
  >++++++[-<------->]+<-- Subtract 44 (comma ASCII code) and set an else flag
  [----                   Result is not null : subtract again 4
    >->+++++++++[-<<<     Update current integer
    [->+>+<<]>>[-<<+>>]   (add it nine times to the actual digit
    >]<<[-<+>]            then this whole block to the current integer)
  ]>[                     Result is null
    ->                    Move to start a new number
  ]<
,]                        Read an loop
```

#Minified version
```
>>,[>++++++[-<------->]+<--[---->->+++++++++[-<<<[->+>+<<]>>[-<<+>>]>]<<[-<+>]]>[->]<,]
```

# Final state

* Memory: 0, _numbers_, 0
* Cursor: on final 0
* Input: empty
* Output: unchanged

# Test program

This program reads comma-separated integer values, and then print the corresponding ASCII chars

```
>>,[>++++++[-<------->]+<--[---->->++++++++   read comma separated values
+[-<<<[->+>+<<]>>[-<<+>>]>]<<[-<+>]]>[->]<,]   ** part 2 **
<[<]>[.>]                                     Go back to the first value, and print them all as chars
```

Try with 66,114,97,105,110,70,117,99,107,32,114,111,99,107,115,32,33,33,33 for example :)
