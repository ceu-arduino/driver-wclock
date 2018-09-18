# Wall-Clock Functionality

Control the passage of time from the real world measured in *seconds*,
*microseconds*, etc.

## API

### Includes

```
#include "wclock.ceu"
```

### Output

#### WCLOCK_NOW

Gets the number of elapsed microseconds since the board reset.

```
output &u32 WCLOCK_NOW;
```

Parameters:

- `&u32`: reference to write the value

*NOTE: Only works if a time unit await is active.*

*NOTE: The number overflows around every 4 seconds.*

### WCLOCK_FREEZE

Freezes the whole program for a given number of microseconds.

```
output u32 WCLOCK_FREEZE;
```

Parameters:

- `u32`: number of milliseconds

### Input

#### Time units

Awaits for units of time: `s`, `ms`, `us`.

See also:
    - https://ceu-lang.github.io/ceu/out/manual/v0.30/statements/#timer

## Examples

### Periodic Switching

Blinks an LED connected to pin 13:

```
#include "out.ceu"
#include "wclock.ceu"

output high/low OUT_13;

loop do
    emit OUT_13(on);
    await 1s;
    emit OUT_13(off);
    await 1s;
end
```

### Print Current Time

Print current time on every second:

```
#include "wclock.ceu"

{ Serial.begin(9600); }

loop do
    await 1s;
    var u32 us = _;
    emit WCLOCK_NOW(&us);
    {
        Serial.print("MS ");
        Serial.println(@us/1000);
    }
    emit WCLOCK_FREEZE(10000);
end
```
