Nelder-Mead Minimization Algorithm
==================================

This repository consists of a modern C++ port of the original implementation
of the Nelder-Mead algorithm to minimize a scalar function of several variables.

The algorithm itself was proposed by John Nelder and Roger Mead in 1965
[[1]](https://doi.org/10.1093/comjnl/7.4.308).
The original implementation was created for Fortran77 by R. O’Neill in 1971
[[2]](https://doi.org/10.2307/2346772)
with subsequent remarks by Chambers & Ertel [[3]](https://doi.org/10.2307/2347015),
Benyon [[4]](https://doi.org/10.2307/2346539)
and Hill [[5]](https://doi.org/10.2307/2347187).
The implementation was subsequently ported to C by John Burkardt.

This port to modern C++ takes the same set of parameters and displays
**the same numerical behaviour as the original routine**.
Changes were restricted to secondary issues:
* output variables moved to returned struct type
* function is now passed as std::function which allows using objects as well as traditional functions
* floating-point type and number of variables are now template arguments
* std::array is now used instead of pointers to raw arrays
* overall comments and code formatting

All these changes are contained in
[this changeset](https://github.com/develancer/nelder-mead/commit/b159c43f50f3a7dcaeb1d2c119b944d216dcf9ca).

## Requirements

Any C++ compiler with support for the C++11 standard will do.

## How to use

Simply include the header file in your program:
```C++
#include "nelder_mead.h"
```
and call the `nelder_mead` function with your own function(al) as a
first parameter, e.g.
```C++
double function_to_minimize(const std::array<double,2>& x) {
    return x[0]*x[0] + x[1]*x[1];
}

std::array<double,2> start = { 0.5, 0.5 };
std::array<double,2> step = { 0.1, 0.1 };

nelder_mead_result<double,2> result = nelder_mead<double,2>(
    function_to_minimize,
    start,
    1.0e-25, // the terminating limit for the variance of function values
    step
);
std::cout << "Found minimum: " << std::fixed << result.xmin[0] << ' ' << result.xmin[1] << std::endl;
```
You can use an ordinary C-like function as well as any object which has the
`operator()` taking the `std::array` as an argument.

If your function takes a raw `double*` pointer instead of `std::array`, you
can use a simple lambda-style wrapper, e.g.
```C++
nelder_mead<double,2>(
    [](const std::array<double,2>& x) {
        return function_to_minimize(x.data());
    },
    ...
```

## License

Copyright Ⓒ 2021 O’Neill, Burkardt, Różański.
This code is distributed under the GNU LGPL license
which can be found in a text file named *LICENSE* in this repository.

## References

[1] Nelder, J. A., & Mead, R. (1965), A Simplex Method for Function Minimization.
The Computer Journal, 7(4), 308–313. https://doi.org/10.1093/comjnl/7.4.308

[2] O’Neill, R. (1971). Algorithm AS 47:
Function Minimization Using a Simplex Procedure.
Journal of the Royal Statistical Society. Series C (Applied Statistics),
20(3), 338–345. https://doi.org/10.2307/2346772

[3] Chambers, J. M., & Ertel, J. E. (1974). Remark AS R11:
A Remark on Algorithm AS 47 “Function Minimization using a Simplex Procedure.”
Journal of the Royal Statistical Society. Series C (Applied Statistics),
23(2), 250–251. https://doi.org/10.2307/2347015

[4] Benyon, P. R. (1976). Remark AS R15:
Function Minimization Using a Simplex Procedure.
Journal of the Royal Statistical Society. Series C (Applied Statistics),
25(1), 97–97. https://doi.org/10.2307/2346539

[5] Hill, I. D. (1978). Remark AS R28: A Remark on Algorithm AS 47:
Function Minimization Using a Simplex Procedure.
Journal of the Royal Statistical Society. Series C (Applied Statistics),
27(3), 380–382. https://doi.org/10.2307/2347187
