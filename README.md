# APL Idioms & Solutions
Solutions of various problems in Dyalog APL  
**Note:** You can try all the code below at [Try APL](tryapl.org).

## Solutions List
- FizzBuzz (upto 100) - `{⊃(1+2⊥0=5 3|⍵)⌷⍵ 'Fizz' 'Buzz' 'FizzBuzz'}¨⍳100`
- Swastika Symbol - `' -|'[1+3∘.((0 2∊⍨-⍨)×(1+2|⊣))⍥⍳5]`

### Algebra
- Function to evaluate a polynomial at a value - `{(⌽⍵)+.×⍺*¯1+⍳≢⍵}` where:
    - `⍵` (right argument) is an array of coefficients of polynomial (highest power to lowest (constant) power)
    - `⍺` (left argument) is value at which polynomial is to be evaluated.
 
 - Function to compare two arrays by priority - `×1↑0,⍨(0~⍨-)`, i.e.,
   first compare first elements, then second elements, and so on until the arrays diverge.
   The result is `1` (Left > Right), `¯1` (Left < Right) or `0` (Left = Right).

### Statistics - APL Functions
**Note:** Unless otherwise noted, the inputs to all listed functions are 1-D Arrays.

#### Monadic (Single Argument) Functions
- Arithmetic Mean / Average - `avg ← +/÷≢`
- Geometric Mean - `gmean ← ×/*(÷≢)`
- Harmonic Mean - `hmean ← {÷+/÷⍵}×≢`
- Variance - `var ← (+/2*⍨⊢-avg)÷(¯1+≢)`
- Standard Deviation / RMS (Root Mean Square) - `stddev ← .5*⍨var`

#### Dyadic (Two Argument) Functions
**Note:** Each function below has left argument Frequencies, right argument Data. Both arguments are 1-D arrays.

- Inner Product / Weighted Mean / Arithmetic Mean for Sample Proportions - `ip ← +.×`
- Variance for Sample Proportions - `varsample ← +.× ∘ ((2*⍨⊢-avg)⊢)`
