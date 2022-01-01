# APL Idioms & Solutions
Solutions of various problems in **Dyalog APL**  
**Note:** You can try all the code below at [Try APL](tryapl.org).

## Basic Idioms
- `↑` (Mix) - converts a vector of vectors into a single matrix of scalars
- `⍕` (Format / Round) right argument to N decimal places, where N is left argument. If N=0, then this is same as finding nearest integer to number.

### String Functions
- `⊢⊂⍨1,' '∘=` - function to split string into words

### Complex Numbers
```
a ← 1j2    ⍝ Complex No. (Real = 1, Imaginary = 2)
9○a        ⍝ Get Real Part
11○a       ⍝ Get Imaginary Part
```
This is the [Circle Operator](https://help.dyalog.com/18.2/Content/Language/Symbols/Circle.htm), which can be used to perform these and other trignometric operations.

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

## Misc
- FizzBuzz function (array upto given argument) - `{⊃(1+2⊥0=5 3|⍵)⌷⍵ 'Fizz' 'Buzz' 'FizzBuzz'}¨⍳`
- Swastika Symbol - `' -|'[1+3∘.((0 2∊⍨-⍨)×(1+2|⊣))⍥⍳5]       ⍝ 3 5⍴'| |   - -   | |'`
- `{((⊢<¯1∘↑)+\⍵)/⍵}` - function that takes a boolean array and strips the last 1 (followed by all 0s)
