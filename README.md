# APL Solutions
Solutions of various problems in Dyalog APL

## Solutions List
- FizzBuzz (upto 100) -> `{⊃(1+2⊥0=5 3|⍵)⌷⍵ 'Fizz' 'Buzz' 'FizzBuzz'}¨⍳100`

### Statistics - APL Functions
**Note:** Unless noted otherwise, each function below takes a 1-D array of numbers as input.

- Arithmetic Mean - `+/÷≢`
- Geometric  Mean - `×/*(÷≢)`
- Harmonic   Mean - `{÷+/÷⍵}×≢`
