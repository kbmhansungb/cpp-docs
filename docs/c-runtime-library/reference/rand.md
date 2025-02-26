---
title: "rand"
description: "API reference for rand, which generates a pseudorandom number by using a well-known and fully reproducible algorithm."
ms.date: "7/7/2021"
api_name: ["rand", "_o_rand"]
api_location: ["msvcrt.dll", "msvcr80.dll", "msvcr90.dll", "msvcr100.dll", "msvcr100_clr0400.dll", "msvcr110.dll", "msvcr110_clr0400.dll", "msvcr120.dll", "msvcr120_clr0400.dll", "ucrtbase.dll", "api-ms-win-crt-utility-l1-1-0.dll", "ntoskrnl.exe", "api-ms-win-crt-private-l1-1-0.dll"]
api_type: ["DLLExport"]
topic_type: ["apiref"]
f1_keywords: ["STDLIB/rand", "rand"]
helpviewer_keywords: ["generating pseudorandom numbers", "random numbers, generating", "numbers, pseudorandom", "rand function", "pseudorandom numbers", "numbers, generating pseudorandom"]
---
# `rand`

Generates a pseudorandom number. A more programmatically secure version of this function is available; see [`rand_s`](rand-s.md). Numbers generated by **`rand`** aren't cryptographically secure. For more cryptographically secure random number generation, use [`rand_s`](rand-s.md) or the functions declared in the C++ Standard Library in [`<random>`](../../standard-library/random.md).

## Syntax

```C
int rand(void);
```

## Return Value

**`rand`** returns a pseudorandom number, as described above. There's no error return.

## Remarks

The **`rand`** function returns a pseudorandom integer in the range 0 to **`RAND_MAX`** (32767). Use the [`srand`](srand.md) function to seed the pseudorandom-number generator before calling **`rand`**.

The **`rand`** function generates a well-known sequence and isn't appropriate for use as a cryptographic function. For more cryptographically secure random number generation, use [`rand_s`](rand-s.md) or the functions declared in the C++ Standard Library in [`<random>`](../../standard-library/random.md).

By default, this function's global state is scoped to the application. To change this, see [Global state in the CRT](../global-state.md).

## Requirements

|Routine|Required header|
|-------------|---------------------|
|**`rand`**|`<stdlib.h>`|

For more compatibility information, see [Compatibility](../../c-runtime-library/compatibility.md).

## Example

```C
// crt_rand.c
// This program seeds the random-number generator
// with a fixed seed, then exercises the rand function
// to demonstrate generating random numbers, and
// random numbers in a specified range.

#include <stdlib.h> // rand(), srand()
#include <stdio.h> // printf()

void SimpleRandDemo(int n)
{
    // Print n random numbers.
    for (int i = 0; i < n; i++)
    {
        printf("  %6d\n", rand());
    }
}

void RangedRandDemo(int range_min, int range_max, int n)
{
    // Generate random numbers in the interval [range_min, range_max], inclusive.

    for (int i = 0; i < n; i++)
    {
        // Note: This method of generating random numbers in a range isn't suitable for
        // applications that require high quality random numbers.
        // rand() has a small output range [0,32767], making it unsuitable for
        // generating random numbers across a large range using the method below.
        // The approach below also may result in a non-uniform distribution.
        // More robust random number functionality is available in the C++ <random> header.
        // See https://docs.microsoft.com/cpp/standard-library/random
        int r = ((double)rand() / RAND_MAX) * (range_max - range_min) + range_min;
        printf("  %6d\n", r);
    }
}

int main(void)
{
    // Seed the random-number generator with a fixed seed so that
    // the numbers will be the same every time we run.
    srand(1792);

    printf("Simple random number demo ====\n\n");
    SimpleRandDemo(10);
    printf("\nRandom number in a range demo ====\n\n");
    RangedRandDemo(-100, 100, 100000);
}```

```Output
Simple random number demo ====

    5890
    1279
   19497
    1207
   11420
    3377
   15317
   29489
    9716
   23323

Random number in a range demo ====

     -82
     -46
      50
      77
     -47
      32
      76
     -13
     -58
      90
```

## See also

[Floating-Point Support](../../c-runtime-library/floating-point-support.md)\
[`srand`](srand.md)\
[`rand_s`](rand-s.md)\
[C++ `<random>` library](../../standard-library/random.md)
