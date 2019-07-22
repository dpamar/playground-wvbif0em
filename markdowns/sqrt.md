# Square root, part 1 : Newton's method

_Well, ok, it's not square root, it's integer part of square root_

In most programming languages, this integer square root (iSqrt) is generally computed using the very fast Newton's method.

Let's have a function f, its derivate f'; and we want to find z so that f(z) = 0. For any start value value _x0_ we can define _x1_ = _x0_ - f(_x0_)/f'(_x0_), then x2, x3, ...

Newton's method states that for a 'good' x0, xN values will converge to z.

So, how can it help us ? Define f(x) = x^2 - n : f(sqrt(n)) = 0. f'(x) = 2x. And X-f(X)/f'(X) = X-(X^2-n)/2X = X/2 + n/2X = (X+n/X)/2

Example : I want find square root of 250. I start with half this value : 125. Then
* 125 ==> (125+250/125)/2 = 63.5 ==> 63
* 63 ==> (63+250/63)/2 = 33.48 ==> 33
* 33 ==> (33+250/33)/2 = 20.28 ==> 20
* 20 ==> (20+250/20)/2 = 16.25 ==> 16
* 16 ==> (16+250/16)/2 = 15.8 ==> 15
* 15 ==> (15+250/15) = 15.8 ==> 15

Stop: answer is 15; and indeed 15^2 < 250 < 16^2

Corner case : 63 (we can start with 63)
* 63 ==> (63+63/63)/2 = 32 ==> 32
* 32 ==> (32+63/32)/2 ==> 16
* 16 ==> (16+63/16) ==> 9
* 9 ==> (9+63/9)/2 ==> 8
* 8 ==> (8+63/8) ==> 7
* 7 ==> (7+63/7) ==> 8

Call the source value X and target value Y, for each step : we cannot stop when X==Y (first case); because it does not work for the second one (it will be 7, 8, 7, 8, 7, 8, ....)

We need to stop when X<=Y (this explains why this post comes after the LE / GT one ^^)

# Let's start

* Memory: N, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
* Cursor: on N 
* Input: any

# Process

* Memory will be N, _loop flag_, _current X_, 0, 0, ....
* Copy N into current X
* Set loop flag to 1
* While loop indicator is set
  * reset loop flag
  * We need X
    * to compute Y (twice, but a single copy will be enough)
    * to compare against Y
    * to be the final result
  * We need N
    * to compute Y
  * So first, create copies of N and X
    * Memory: N, 0, X, 0, X, N, 0, 0, 0, X, 0
  * Divide N/X
    * Memory: N, 0, X, 0, X, 0, R, 0, 0, R', N/X
    * R+R' = X (hence, we can reconstruct our second X needed to compute Y)
  * Compute 2Y first = X+N/X = R + R' + N/X
    * Memory: N, 0, X, 0, X, 0, 0, 0, 0, 0, 2Y
  * Y will be needed
    * to compare against X
    * to be the final result (if needed)
    * Memory: N, 0, X, Y, X, 0, 0, 0, Y, 0, 0
  * Compare
    * Memory: N, 0, X, _result_, 0, 0, 0, 0, Y, 0, 0
  * If result is 1 (X > Y)
    * set loop flag to 1
    * reset X and replace by Y
* Loop
  * Here, X <= Y. Answer is X

_Note_: this algorithm works modulo 256. And not just about N : 2Y as well has to be less than 255 or it will fail (e.g. 255 does not work, because 2Y = 256 = 0)

# Code
```
[->+>+<<]>[-<+>]+                                    Copy N into X and set loop flag
[                                                    While loop flag is set
  -                                                  reset loop flag
  >[->+>+>>>>>+<<<<<<<]>[-<+>]                       Copy X
  <<<[->+>>>>+<<<<<]>[-<+>]                          Copy N
  >>>>[->+>>+>-[<-]<[->>+<<<<[->>>+<<<]>]<<]         Divide N by X (euclidean division)
  >[->>>+<<<]>>>[->+<]                               Build 2Y
  >[-[-<<+<<<<<+>>>>>>]<[>]>]                        Build Y and store twice (note : to divide 2Y by 2: subtract 1 then if not null subract 1 and add 1 to the result and ends up to the same place in both cases)
  <<<<<<<[->>+<[->-]>[<<[-]>>->]<<<]>[[-]<+>]<       Compare Y and X
  [                                                  Y less than X; continue
    <[-]>>>>>>[-<<<<<<+>>>>>>]<<<<<-<<+>>            reset X and replace by Y; reset result; set loop flag
  ]
<<]                                                  loop
```

# Minified version
```
[->+>+<<]>[-<+>]+[->[->+>+>>>>>+<<<<<<<]>[-<+>]<<<[->+>>>>+<<<<<]>[-<+>]>>>>[->+>>+>-[<-]<[->>+<<<<[->>>+<<<]>]<<]>[->>>+<<<]>>>[->+<]>[-[-<<+<<<<<+>>>>>>]<[>]>]<<<<<<<[->>+<[->-]>[<<[-]>>->]<<<]>[[-]<+>]<[<[-]>>>>>>[-<<<<<<+>>>>>>]<<<<<-<<+>>]<<]
```

# Final state

* Memory: N, 0, _iSqrt(n)_, 0, 0, 0, 0, 0, _Y_, 0, 0
* Cursor: after N
* Input: unchanged
* Output: unchanged

# Test program

This program mixes all the previous steps : it reads an integer from the input and writes its iSqrt on the output
```
>,[>++++++[-<-------->]>+++++++++[<<<[->+>+<<]>>[-<<+>>]>-]<<[-<+>],]<   read integer
[->+>+<<]>[-<+>]+[->[->+>+>>>>>+<<<<<<<]>[-<+>]<<<[->+>>>>+<<<<<]>[-<+   iSqrt
>]>>>>[->+>>+>-[<-]<[->>+<<<<[->>>+<<<]>]<<]>[->>>+<<<]>>>[->+<]>[-[-<    ** part 2 **
<+<<<<<+>>>>>>]<[>]>]<<<<<<<[->>+<[->-]>[<<[-]>>->]<<<]>[[-]<+>]<[<[-]    ** part 3 **
>>>>>>[-<<<<<<+>>>>>>]<<<<<-<<+>>]<<]                                     ** part 4 **
>>>>>>>[-]<<<<<<                                                         clear Y
[>>>>++++++++++<<<<[->+>>+>-[<-]<[<<[->>>+<<<]>>>>+<<-<]<                print result
<]++++++++[->++++++<]>[-<+>]>>>>[-<<<<+>>>>]<[-]<<<]<[.<]                 ** part 2 **
```
