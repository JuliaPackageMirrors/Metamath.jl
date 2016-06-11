# Metamath

This package provides a standalone verifier for Metamath database files.

For more information on the Metamath language see: http://us.metamath.org.

The code is written in Julia and inspired by `checkmm.cpp` by Eric Schmidt (see
http://us.metamath.org/downloads/checkmm.cpp).

## Installation

Install this package within Julia using
```julia
Pkg.add("Metamath")
```

## Usage

The package comes with sample Metamath databases in the `data` directory of the
package. To test verification (without downloading anything), try:

```julia
using Metamath
Metamath.main(joinpath(Pkg.dir("Metamath"),"data","demo.mm"))
```

A more interesting database file is `set.mm` (containing axioms and theorems
from Set Theory and beyond). To verify it:
1. Download the updated `set.mm` to the current directory from the Metamath website.
2. Use the following code:

```julia 
using Metamath
Metamath.main("set.mm")
```

There is also a test script which verifies the sample databases which can be run using:

```julia
Pkg.test("Metamath")
```

## Exports and useful internals

The package exports a single function `mmverify!` which accepts a Metamath.Environment
variable and a filename and verifies it. The verification throws a MetamathException
if something goes wrong (with a, hopefully, informative message and a backtrace).

Internally, the function `Metamath.main` verifies a file with a new environment or the package
internal global environment `Metamath.globalenv`.

Redefinition of entities such as constants, variables and axioms is not allowed. So to reuse
a `Metamath.Environment` it may be returned to the initial state with `empty!(env)` (overloaded
from Base).

## Performance

A benchmark test consisting of verification of `set.mm` was conducted on June 11th 2016 with
the latest versions of 4 verifiers. The results are as follows:

| Time (s)  | Verifier    | Computer language         |
| --------: | :---------- | ------------------------- |
| 108.1     | mmverify.py | Python (3.4.3)            |
|  25.2     | MM Tool     | Javascript (V8 5.0.71.49) |
|  15.5     | checkmm.cpp | C++ (g++ 4.8.4)           |
|  10.1     | Metamath.jl | Julia (0.4.6pre)          |

or graphically:

| Verifier    | Bar chart of time                    |
| :---------- | ------------------------------------ |
| mmverify.py | #################################### |
| MM Tool     | ########                             |
| checkmm.cpp | #####                                |
| Metamath.jl | ###                                  |

[![Build Status](https://travis-ci.org/getzdan/Metamath.jl.svg?branch=master)](https://travis-ci.org/getzdan/Metamath.jl)

