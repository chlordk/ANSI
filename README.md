
[![Arduino CI](https://github.com/RobTillaart/ANSI/workflows/Arduino%20CI/badge.svg)](https://github.com/marketplace/actions/arduino_ci)
[![Arduino-lint](https://github.com/RobTillaart/ANSI/actions/workflows/arduino-lint.yml/badge.svg)](https://github.com/RobTillaart/ANSI/actions/workflows/arduino-lint.yml)
[![JSON check](https://github.com/RobTillaart/ANSI/actions/workflows/jsoncheck.yml/badge.svg)](https://github.com/RobTillaart/ANSI/actions/workflows/jsoncheck.yml)
[![GitHub issues](https://img.shields.io/github/issues/RobTillaart/ANSI.svg)](https://github.com/RobTillaart/ANSI/issues)

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/RobTillaart/ANSI/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/RobTillaart/ANSI.svg?maxAge=3600)](https://github.com/RobTillaart/ANSI/releases)
[![PlatformIO Registry](https://badges.registry.platformio.org/packages/robtillaart/library/ANSI.svg)](https://registry.platformio.org/libraries/robtillaart/ANSI)


# ANSI

Arduino library with basic ANSI display codes for terminal applications.


# Description

ANSI codes are special codes that are send to a terminal e.g. VT100 to add
attributes to displayed characters.
Typical examples are bold, blink or colour. ANSI codes are also known as escape codes.
The set of codes is large, however not all terminal types do support all codes.

Sending these ANSI codes to a simple ASCII only terminal like the one in the Arduino
IDE might result in garbage. So use with care.

#### Breaking change 0.2.0

The **gotoXY(x, y)** has changed as the X and Y coordinates were swapped.

The code has been updated to explicitly mention which is row and which is column.
- **gotoXY(uint8_t column, uint8_t row)** 

#### Related

https://en.wikipedia.org/wiki/ANSI_escape_code


## Terminals tested

Tests are done with
- TeraTerm 4.102 + 4.106 (VT100, VT202, VT525 mode)
- Putty 0.71

Other terminal program's exist so please let me know if yours is working too.
If not, please open an issue.


## Operation

See examples


## Interface

```cpp
#include "ansi.h"
```

#### Constructors

- **ANSI(Stream \* stream = &Serial)** wrapper around Serial.
Can be a software serial too.
- **VT100(Stream \* stream = &Serial)** derived class (wrapper)


#### Stream interface

- **int available()** to check if chars are available on the stream.
- **int read()** read a byte from the stream.
- **int peek()** preview the byte in the stream without fetching.
- **void flush()** 

Stream interface also includes print(), println(), write().


#### Character modi

- **void normal()** normal intensity.
- **void bold()** bold or high intensity.
- **void low()** low intensity.
- **void underline()** idem.
- **void blink()** idem.
- **void reverse()** idem.


#### Colour

- **void foreground(uint8_t fgcolor)**
- **void background(uint8_t bgcolor)**
- **void color(uint8_t fgcolor, uint8_t bgcolor)**

Three helpers to map to the nearest colour.
- **uint8_t gray2color(uint8_t gray)**
- **uint8_t grey2color(uint8_t grey)** idem
- **uint8_t rgb2color(uint8_t r, uint8_t g, uint8_t b)**

- https://en.wikipedia.org/wiki/ANSI_escape_code#3-bit_and_4-bit


#### Positioning

- **void clearScreen()** clears screen and sets cursor to 0,0.
- **void clearLine(uint8_t clear = toEnd)** toEnd = 0,
  toStart = 1, entireLine = 2,
- **void home()** set cursor to 0, 0
- **void gotoXY(uint8_t column, uint8_t row)** set cursor to position. 
Note X == row and Y == column. See #13.
- **void cursorUp(uint8_t x)** idem.
- **void cursorDown(uint8_t x)** idem.
- **void cursorForward(uint8_t x)** idem.
- **void cursorBack(uint8_t x)** idem.


#### Experimental

Look into **ansi.h** for experimental functions and notes.

Version 0.1.6 added a number of experimental function that need more testing.
Some are working with Teraterm, others are unclear of fail.
The user can uncomment these and verify if these work with their terminal.


##### Experimental device type

The **int deviceType()** function needs more testing.

See - https://github.com/RobTillaart/ANSI/issues/9

|  Type   |  Description  |
|:-------:|:--------------|
|   -1    |  unknown      |
|    1    |  VT52         |
|    2    |  VT100        |
|    3    |  VT220        |
|  other  |  unknown      |

As always, constructive feedback is welcome.


## Performance

Since 0.1.5 there is some focus on performance.
Using **ansi.print()** and **ansi.println()** for printing text and numbers is
improved a bit since 0.1.4 by adding the private **write(array, length)**.

Since 0.2.0 the (internal) print() statements are replaced by write().
Although it are small improvements these add up.


## Future

#### Must

- improve documentation
  - elaborate interface
  - colour info

#### Should

- test experimental functions
- test more terminal programs (Linux mac)
- add examples
  - DOS emulator?
  - experimental section
- evaluate experimental code

#### Could

- increase functionality
  - which codes are generic / useful ?
- investigate performance.
  - add line buffer in write(c) to improve throughput?
  - need for flush() with line buffer?
- move code from .h to .cpp
- more derived classes

#### Wont

- move static strings to PROGMEM? as defines?
  roughly ~20 bytes progmem for 4 bytes RAM...


## Support

If you appreciate my libraries, you can support the development and maintenance.
Improve the quality of the libraries by providing issues and Pull Requests, or
donate through PayPal or GitHub sponsors.

Thank you,

