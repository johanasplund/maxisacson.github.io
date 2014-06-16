---
layout: post
title: "<code>"!dlrow olleH">:#,_@</code>"
date: 2014-06-16
categories: esolangs
author: Johan Asplund
---
# Esoteric programming languages
An [esoteric programming language](http://esolangs.org/wiki/Main_Page) is a programming language whose goal more often than not is to have obscure syntax and stretch the boundary of computer programming language design. These languages are either developed to explore new concepts, for their weirdness or purely as a joke. Among the most famous languages belongs [*Brainfuck*](http://esolangs.org/wiki/Brainfuck). Consider this Hello World-program written in Brainfuck
```brainfuck
++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++.
```
Brainfuck consists of a *tape* of infinite length with *memory cells* at even distance from each other. The code controls a pointer, which points at one memory cell at a time. The instruction `+` increases the memory cell by one, and the instruction `.` outputs the character represented by the value of the cell currently being pointed at. 

Other esoteric languages include [LOLCODE](http://esolangs.org/wiki/LOLCODE), which is a general purpose esoteric programming language with syntax to resemble the lolcat-jargon. Here follows a version of *99 Bottles of beer* written in LOLCODE
```
HAI
    VISIBLE "O HAI! IM IN UR BEERZ!"
    I HAS A VAR
    LOL VAR R 99
    IM IN YR LOOP
        VISIBLE VAR!
        VISIBLE " BOTTLZ OF BEER N TEH WALL, "!
        VISIBLE VAR!
        VISIBLE " BOTTLZ OF BEER!"
        VISIBLE "TAKE 1 DWN, PAS IT AROUN, "!
        NERFZ VAR!!
        IZ VAR LIEK 0?
            YARLY
                VISIBLE "NO MOAR"!
            NOWAI
                VISIBLE VAR!
        KTHX
        VISIBLE " BOTTLZ OF BEER N TEH WALL!"
        IZ VAR LIEK 0?
            GTFO
        KTHX
    KTHX
    VISIBLE "GIEV MOAR PLZ! KTHXBAI!"
KTHXBYE
```
## Befunge-93
One particular esoteric programming language that I have spent quite a lot of time learning myself, is Befunge-93. Befunge-93 is a two-dimensional language designed in 1993 by [Chris Pressey](http://esolangs.org/wiki/Chris_Pressey) whose syntax may seem extremely obscure at a first glance, but is very pleasant once the initial learning-bump has been passed. Consider the following Hello World-program written in Befunge-93.
```befunge
"!dlrow olleH">:#,_@
```
Now you might think that this does not seem *that* obscure. It is clear that it says *Hello world!* in the code! But now consider the following code
```befunge
0 952**7+    v
&gt;:3-        v6
 &gt;-  :3-::7v:6
 1         -8*
 *         :+  
+8         2  
66 v-+9**25<  
:^     **464< 
^         +8:<

   &gt;        :v
   ^    ,+*88_@
```
This code also outputs Hello world.

Befunge-93 consists of a *play field* which the code occupies, and a *stack* which is a one-dimensional array that can store values. Each character in the code which makes up the play field is an instruction. When the code is executed, an instruction pointer starts in the upper left corner and initially travels right in the code. For each character the pointer encounters, the program executes the instruction bound to the character.

### Instruction list and properties of the language

Character |Description 
 -------- | ---------- 
0-9 | Push this number to the stack
+ | Pop `a` and `b`, then push `a+b`.
- | Pop `a` and `b`, then push `a-b`.
* | Pop `a` and `b`, then push `a*b`.
/ | Pop `a` and `b`, then push `floor(b/a)`, provided that `a` is not zero.
% | Pop `a` and `b`, then push `a (mod b)`.
! | Pop `a`. If `a = 0`, push 1, otherwise push 0.
` | Pop `a` and `b`, then push 1 if `b > a`, otherwise 0.
&gt; | Move right.
< | Move left.
^ | Move up.
v | Move down.
? | Move in a random direction.
_ | Pop `a`. If `a = 0`, move right, otherwise move left.
&verbar; | Pop `a`. If `a = 0` , move down, otherwise move up.
" | Start string mode. Push each characters ASCII value all the way up to the next ".
: | Duplicate value on top of the stack.
\ | Swap the two values on top of the stack.
$ | Remove the value on top of the stack.
. | Pop `a`. Output the integer value of `a`.
, | Pop `a`. Output `chr(a)`.
&#35; | Skip next cell.
p | Pop `y`, `y` and `v`. Change the character in position `(x,y)` to `chr(v)`.
g | Pop `y` and `x` , the push the ASCII value of the character in position `(x,y)`.
& | Prompt user for a number and push it.
~ | Prompt user for a character and push its ASCII value.
@ | End program

A basic infinite loop is literally a loop.
```befunge
&gt;v
^<
```

Any character not in the above list is ignored. The play field in Befunge-93 must formally be less than 80 x 25 characters, and has toroidal topology. This means that if the cursor goes off to the right, the pointer comes back on the left side. The stack size is also finite, which means that Befunge-93 is not Turing complete.

There are several other languages in the so called *Funge-family*. Among Befunge-93 there are Befunge-98 which is a more complex version of Befunge-93, which has more instructions and is Turing complete as the finiteness restrictions on the play field and stack are suppressed. The closest relatives to Befunge are Unefunge and Trifunge, which are the one- and three-dimensional cousins to Befunge. The most recent successor of Befunge-93 is [Funge-98](http://catseye.tc/node/Funge-98)

### Some basic programs

The simplest Hello World-program is probably along the lines of the following
```befunge
"!dlroW olleH",,,,,,,,,,,,@
```
It first pushes the string "Hello World!" to the stack, backwards. This is because the instruction `,` outputs the value currently at the top of the stack. We thus have to read in the word so that the first letter in "Hello World!" ends up at the top of the stack. For the slightly more elegant program
```befunge
"!dlrow olleH">:#,_@
```
We first read the string "Hello World!" (backwards, since we output values in the stack from the top). Then comes a quite elegant loop, which we can break down as follows. When the pointer points at `>` for the first time the ASCII value of H (72), is at the top of the stack. 
Character | Value 
--------- | ------
H|72
The next instruction is `:`, and this instruction duplicates the value 72.
Character | Value 
--------- | ------
H|72
H|72
Then we encounter the so called *trampoline* which skips the `,` and goes on to `_`. The instruction `_` pops the value 72 and checks whether it is 0 or not. It is not, so the pointer is told to move to the left, which outputs a H when it encounters the `,`. The pointer is then on the trampoline again, which jumps it to the `>` which essentially continues the loop. The program will do this for as long as the stack is not empty. If an instruction tries to pop a value from an empty stack, a 0 will be pushed. So the pointer will only go right at the `_` when the stack is empty, and no character is left to output.

The most interesting instructions are `g` and `p`. These instructions can be used to modify the code, while it is running. If one wishes to write more complex programs, the `g` and `p` are used to store values on the play field. One program that takes advantage of this, is the following bubblesort I implemented in Befunge.
```befunge
vbubblesort
 1                           v_@#--"/"g11:+1<       
 0                           >          :0g,^            
v      p21"0"p11+1g11p0+1-"0"1#g11g12p0-"0"g11g13 <
&gt;11g"0"-0g:" "-!#v_:21p11g1+#$"0"-0g:" "-!#v_:31p`|
^   p21"0"$p11"1"<          _^#  -1-g21g11$<   
^                                 p21+1g21p11+1g11<
```
The string to the right, which is to the right of the v in the upper left corner is the input string. This program takes the string `bubblesort` (in this case), and outputs the sorted string `bbbelorstu`. The 1 and 0 below the input string are variables used in the program to keep track of when the string actually is sorted or not.

### An interpreter

Having spent some time with Befunge-93 to familiarize myself with Befunge-93 I decided to write an interpreter. While learning Befunge-93 I used an online interpreter created by [Alexios Chouchoulas](http://www.bedroomlan.org/tools/befunge-93-playground), and this was my inspiration. I ended up with an [interpreter written in Python with pygame](https://github.com/johanasplund/befunge-93). Here is a sample run of the following code, which computes the factorial of a given number.
```befunge
&>:1-:#v_$>*v@.$<
 ^     <    >\:!|
          ^     <
```
![src](/assets/befunge/factorial.gif)

### More than just code

When designing Befunge-code it is much like pulling cables along a big plane. One could control the flow in a number of different ways, and there are many different approaches and graphical styles one can use to design programs in Befunge. This graphical aspect adds another dimension and Befunge as a whole offers a different way of thinking about programming, which is quite satisfying for a esoteric programming language. I dare to say that the favorite program I have (literally) designed in Befunge is the following 99 bottles song.
```befunge
<v,,,,,,,,,,,," "       "on the wall, ",+7,-7   ,-4,-+92_#v,# <" bo"    "ttles of milk".:<"c"    
,<v"b"          "o"     "ttle"                  "s of"                  " mil"        "k".:, 
<v<v+1           9,+    7,-7,-                  4,-+92                  ,,,,,,         ,,,,,, 
^"v<v<           3+8    2,+1*:                  +55,+7                  *:+46,         +1"`"< 
^ ,v<v           +*5    2*:+55                  ,++29*                  :+91<<         <<<>v^ 
^"7*v<           >2*    2+,82+                  :*,"7"                  2*1+,v         >+^,>^ 
^<32,X          >^v     +1"`",                  +2*2+7                  8,,+<>       7v*>*^ 
v<*+>55*4*1+,96+^v>,    73+::*+,52*:*,v         ", ",,XX^<v#<<<         <XXX^7****2222<:4
<^<,>2        +,$1-:    .64+3*                  2+,"o"                   "b",,     55+v:^
+37<*X         ######   Xv3+73                  ,,:*5<                  ^v:*4+     9*2<^<
XXXX^2         *53,*<   X>*2+v                  >2*5+^                  ^>,,##     " fo"# v
>64+:*         ,492+^   v*37,<                  ^9,+1<                  ^v,,,,      "les "<
^,+*::         :+73<X   >5*,7v                  >,"`"^                  ^>,,,#       "reeb"v
>"uor"        ,,,XX^    v*:+3<                  ^-+66<                  !v:+91        #,,,,<
^,+1"`",+2**352,++79    <#############>337+*2   +,"|"^####>:91+:*9+->   ^>,,:#        ###v_@?
```
This program shows the extreme flexibility of Befunge in how one can arrange the code to work in your favor. Besides the fact that the code spells out BEER in ASCII art, the code is slightly obfuscated. The top row of the R clearly reads "bottles of milk", but the code actually outputs "beer".

