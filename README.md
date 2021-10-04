# APL Solutions
Solutions of various problems in Dyalog APL

## Solutions List
- FizzBuzz (upto 100) -> `{⊃(1+2⊥0=5 3|⍵)⌷⍵ 'Fizz' 'Buzz' 'FizzBuzz'}¨⍳100`

### Statistics - APL Functions
**Note:** Unless otherwise noted, the inputs to all listed functions are 1-D Arrays.

#### Monadic (Single Argument) Functions
- Arithmetic Mean - `avg ← +/÷≢`
- Geometric Mean - `gmean ← ×/*(÷≢)`
- Harmonic Mean - `hmean ← {÷+/÷⍵}×≢`
- Variance - `var ← (+/2*⍨⊢-avg)÷(¯1+≢)`
- Standard Deviation / RMS (Root Mean Square) - `stddev ← .5*⍨var`

#### Dyadic (Two Argument) Functions
**Note:** The values given for each function are (Left,Right) arguments to the function. Both are 1-D arrays.

- Inner Product / Weighted Mean / Arithmetic Mean for Sample Proportions - `ip ← +.×` with inputs (Frequency, Values)
- Variance for Sample Proportions - `varsample ← {⍺ +.× 2*⍨(⊢-avg)⍵}` with inputs (Frequency, Values)
