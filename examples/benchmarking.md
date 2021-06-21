---
title: Benchmarking
---

Suppose we write a simple program that sums the squares of some integers:

```futhark
let main (n: i32): i32 =
  reduce (+) 0 (map (**2) (0..<n))
```

We can benchmark this program by adding a test stanza to the source
file (say, `sumsquares.fut`) that looks like this:

    -- ==
    -- compiled input {       1000 } output {   332833500 }
    -- compiled input {    1000000 } output {   584144992 }
    -- compiled input { 1000000000 } output { -2087553280 }

The `output` parts are optional.  We then use `futhark bench`:

```
$ futhark bench sumsquares.fut
Compiling src/sumsquares.fut...
Results for src/sumsquares.fut:
dataset #0 ("1000i32"):             0.20us (avg. of 10 runs; RSD: 2.00)
dataset #1 ("1000000i32"):        290.00us (avg. of 10 runs; RSD: 0.03)
dataset #2 ("1000000000i32"):  270154.20us (avg. of 10 runs; RSD: 0.01)
```

This will use the default (`c`) backend.  Things are more
interesting with `opencl`:

```
$ futhark bench --backend=opencl sumsquares.fut
Compiling src/sumsquares.fut...
Results for src/sumsquares.fut:
dataset #0 ("1000i32"):            49.70us (avg. of 10 runs; RSD: 0.18)
dataset #1 ("1000000i32"):         44.40us (avg. of 10 runs; RSD: 0.02)
dataset #2 ("1000000000i32"):    1693.80us (avg. of 10 runs; RSD: 0.04)
```

# See also

The [manpage for `futhark
bench`](https://futhark.readthedocs.io/en/stable/man/futhark-bench.html).