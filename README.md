# APL Idioms & Solutions
Solutions of various problems in **Dyalog APL** 
**Note:** You can try all the code below at [Try APL](tryapl.org).

## Solutions List
- FizzBuzz (upto 100) - `{⊃(1+2⊥0=5 3|⍵)⌷⍵ 'Fizz' 'Buzz' 'FizzBuzz'}¨⍳100`
- Swastika Symbol - `' -|'[1+3∘.((0 2∊⍨-⍨)×(1+2|⊣))⍥⍳5]`

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
- Function to evaluate a polynomial at a value - `{(⌽⍵)+.×⍺*¯1+⍳≢⍵}` where:
    - `⍵` (right argument) is an array of coefficients of polynomial (highest power to lowest (constant) power)
    - `⍺` (left argument) is value at which polynomial is to be evaluated.
  **Note:** This might be inefficient, since calculating power for each index is expensive.
  Instead it might be better to do something similar to this Haskell: `iterate (2*) 1`.
  

 - Function to compare two arrays by priority - `×1↑0,⍨(0~⍨-)`, i.e.,
   first compare first elements, then second elements, and so on until the arrays diverge.
   The result is `1` (Left > Right), `¯1` (Left < Right) or `0` (Left = Right).

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


