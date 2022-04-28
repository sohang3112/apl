# APL Idioms & Solutions
Solutions of various problems in **Dyalog APL**  

## Resources
You can try most of the code below at [Try APL](tryapl.org).

### Web Books
- [Mastering Dyalog APL](https://mastering.dyalog.com)
- [Learn APL](https://xpqz.github.io/learnapl)
- [APL Cultivations](https://xpqz.github.io/cultivations) is another good book (by the same author)

## Workspaces
- `⎕CY 'dfns'` is equivalent to `from dfns import *` in Python - i.e., import everything unqualified from given workspace.
- [Dfns Workspace](https://aplwiki.com/wiki/Dfns_workspace) is built-in, has many useful functions.
```
⎕CY 'dfns'   ⍝ Import built-in workspace 'dfns'
2 pco 30     ⍝ Prime Factorization of 30, using 'pco' function from 'dfns'
```    

## Benchmarking / Profiling
- `]PERFORMANCE.runtime '10?10'` - measure execution time of the code (which is written in a string)
- `⎕AI` - gives compute time since start of APL session (along with other information)
- `∘.(=×⊢)⍨⍳N` is ~200x slower than `A ← N N⍴0 ⋄ A[,⍨¨⍳N] ← ⍳N` for creating a [Diagonal Matrix](https://en.wikipedia.org/wiki/Diagonal_matrix) for `N←1000` - I measured this with `]PERFORMANCE.runtime`!

## Basic Idioms
- `↑` (Mix) - converts a vector of vectors into a single matrix of scalars
- `⍕` (Format / Round) right argument to N decimal places, where N is left argument. If N=0, then this is same as finding nearest integer to number.

### Matrix
- [Laminate (comma with fractional axis)](https://mastering.dyalog.com/Working-on-Data-Shape.html) can:
    - Join vectors into matrix as rows - `A,[0.5]B`
    - Join vectors into matrix as columns - `A,[1.5]B`
    - Also works for higher dimensions (see link).

### String Functions
- `⊢⊂⍨1,' '∘=` - function to split string into words
- `+/∘.=` - Letter frequencies of some characters (left argument) in a string (right argument) (similar to Python's `collections.Counter` class):

### Complex Numbers
```
a ← 1j2    ⍝ Complex No. (Real = 1, Imaginary = 2)
9○a        ⍝ Get Real Part
11○a       ⍝ Get Imaginary Part
```
This is the [Circle Operator](https://help.dyalog.com/18.2/Content/Language/Symbols/Circle.htm), which can be used to perform these and other trignometric operations.

**Note:** 
- Passing a negative number as left argument gives inverse of ordinary function (eg. sine becomes inverse sine). 
- **Example:** `11○a` gives imaginary component of `a`, so `¯11○a` "puts back" real `a` into imaginary component. In other words, `¯11○a` is the same as multiplying `a` with iota `0J1`.

### Date & Time
See the [reference](https://dfns.dyalog.com/n_Dates.htm).

```
 ⎕CY'dfns'                ⍝ load date-time library
 ⎕TS timestamp 'Now'      ⍝ get current timestamp, and format it
 ```
 #### Time of Day
 ```
 Text  ← 'night' 'evening' 'afternoon' 'morning'
 Hours ← 19 18 12    ⍝ starting hours corresponding to the times of day in above variable Text
                     ⍝ starting hour (0) of morning is omitted because it is not required
 timeOfDay ← ⊃Text⌷⍨(1⍳⍨Hours≤⊢)   ⍝ function that takes hour (0-23) as input, returns string (time of day)
 ```
 
### Algebra
- Primary Diagonal of a Matrix - `{⍵[,⍨¨⍳≢⍵]}`

- Sum of Vector Magnitudes - `.5+.*⍨2+.*⍨⊢`
     where (single) argument is a 2D Matrix whose each row is one vector.

- Solve System of Linear Equations - `(⌹⊢)+.×⊣` (uses Matrix Inverse Operator `⌹`)
    - right argument is coefficient matrix
    - left argument is vector of equation constants

- Function to evaluate a polynomial at a value - `⊤⍣¯1` where:
    - right argument is an array of coefficients of polynomial (highest power to lowest (constant) power)
    - left argument is value at which polynomial is to be evaluated. 

 - Function to compare two arrays by priority - `×1↑0,⍨(0~⍨-)`, i.e.,
   first compare first elements, then second elements, and so on until the arrays diverge.
   The result is `1` (Left > Right), `¯1` (Left < Right) or `0` (Left = Right).
   
  - `{∘.(=×⍵⌷⍨⊢)⍨⍳≢⍵}` - function to create [diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix) using array

### Computer Network
- Hemming Distance - `+.(|-)` 
     - Number of bits where two binary sequences differ. 

### Statistics - APL Functions
**Note:** Unless otherwise noted, the inputs to all listed functions are 1-D Arrays.

#### Monadic (Single Argument) Functions
- Frequency Count - `{⍺ (≢⍵)}⌸` (returns 2D matrix whose first column is unique elements, and second column is their frequencies)
    - See explanation for Key Operator ⌸ [here](https://xpqz.github.io/learnapl/key.html).
- Arithmetic Mean / Average - `avg ← +/÷≢`
- Running Average - `+\÷(⍳≢)`
- Geometric Mean - `gmean ← ×/*(÷≢)`
- Harmonic Mean - `hmean ← {÷+/÷⍵}×≢`
- Variance - `var ← (2+.*⍨⊢-avg)÷¯1+≢`
- Standard Deviation / RMS (Root Mean Square) - `stddev ← .5*⍨var`

#### Dyadic (Two Argument) Functions
**Note:** Each function below has left argument Frequencies, right argument Data. Both arguments are 1-D arrays.

- Inner Product / Weighted Mean / Arithmetic Mean for Sample Proportions - `ip ← +.×`
- Variance for Sample Proportions - `varsample ← +.× ∘ ((2*⍨⊢-avg)⊢)`
- Sigmoid Function - `sigmoid ← {÷1+*-⍵÷⍺}`
     - Right Argument `⍵` is actual input
     - Left Argument `⍺` is called **Temperature** (just a mathematical term!)

### Operators (Higher Order Functions) - take functions as argument
- Stochastic / Probability Function - `{(?0⍴⍨⍵)≥⍺⍺⍳⍵}` - output 1 or 0 with probability given by Probability Function `⍺⍺` (input)
   - **Example** - `10∘sigmoid{(?0⍴⍨⍵)≥⍺⍺⍳⍵}10`

## Plotting / Graphing
- `]plot` - plots a vector on Y Axis, index on X Axis. Plot is continous by default.
- **Example** - Plot sigmoid function with 100 data points - `]plot sigmoid ⍳100`

## File I/O
- [Parsing Files - Text, CSV, JSON, XML, HTTP](https://xpqz.github.io/learnapl/io.html)
- Change Working Directory - `]CD 'directory-path-here'`

## Misc
- Format complex number as its real and imaginary parts seperated by a space - `⊃9 11(⊣,' ',⊢)⍥⍕.○⊢`
    - **Example:** `1j¯2` becomes the string `'1 ¯2'`.
- Add random noise to an array - `⊢+∘?0⍴⍨≢` (**Note:** Here, *random noise* means a random number between 0 and 1 is added to each element of array.)
- FizzBuzz function (array upto given argument) - `{⊃(1+2⊥0=5 3|⍵)⌷⍵ 'Fizz' 'Buzz' 'FizzBuzz'}¨⍳`
- Swastika Symbol - `' -|'[1+3∘.((0 2∊⍨-⍨)×(1+2|⊣))⍥⍳5]       ⍝ 3 5⍴'| |   - -   | |'`
- `{((⊢<¯1∘↑)+\⍵)/⍵}` - function that takes a boolean array and strips the last 1 (followed by all 0s)
- `↑(⊢,2∘*)⊂-⍳10` - table of negative powers of 2
- `N←50 ⋄ A←N N⍴' ' ⋄ A[(⌈N×9 11,.○⊢)¨*0j1×○(⍳N-1)÷2×N] ← '*' ⋄ A` - Prints an approximate quarter circle in a 50x50 grid.
- `A,[0.5]'-'` - Underlines string `A` using [Laminate](#matrix).
- Function that shows number spiral:
```
    spiral  ← {A ← ⍵ ⍵⍴' ' ⋄ B ← ¯1↓(9 11,.○⊢)¨+\(1j1×1+⌊⍵÷2),(⊃(⍴∘1¨2/⍳),.×1 0J1 ¯1 0J¯1⍴⍨2∘×)⍵-1 ⋄ A[B] ← 2 0∘⍕¨⍳≢B ⋄ A}
    spiral 5
 20  19  18  17 
  7   6   5  16 
  8   1   4  15 
  9   2   3  14 
 10  11  12  13 
```
- Function that puts boundary around matrix (right argument) using character (left argument):
```
       bounded ← {A←(2+s←⍴⍵)⍴⍺ ⋄ A[1+⍳s]←⍵ ⋄ A}
      '#' bounded ?4 3⍴10
#  # #  # #
# 10 5  9 #
#  4 2 10 #
#  3 8  9 #
#  3 3  6 #
#  # #  # #
```

## Untested
- `(⎕NEW 'Bitmap' (⊂'File' 'image-filename')).CBits` - Read an image's bitmap. Doesn't work on Linux. Supposed to work on Windows, but haven't tested it yet.
