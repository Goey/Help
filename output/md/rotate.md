<div class="section">

<div class="titlepage">

<div>

<div>

#### <span id="rotate"></span>Rotate

</div>

</div>

</div>

<span class="strong">**Syntax:**</span>

``` screen
    Rotate variable {Left | Right} [Simple]
```

<span class="strong">**Command Availability:**</span>

Available on all microcontrollers.

<span class="strong">**Explanation:**</span>

The `Rotate` command will rotate `variable` one bit in a specified
direction. The bit shifted will be placed in the Carry bit of the Status
register (`STATUS.C`). `STATUS.C` acts as a ninth bit of the variable
that is being rotated.

`variable` supports Bytes, Word and Long variables.

When a variable is <span class="strong">**rotated right**</span>, the
bit in the `STATUS.C` location is placed into the MSB of the variable
being rotated, and the LSB of the variable is placed into STATUS.C
location.

When <span class="strong">**rotated left**</span> the opposite occurs.
The MSB of the variable is shifted to the `STATUS.C` bit and the LSB of
the variable will contain what was previously in the `STATUS.C` bit
location.

This table shows the operation of the `Rotate Left` command

<div class="informaltable">

| <span class="strong">**Command**</span> | <span class="strong">**<span class="emphasis">*variable*</span>**</span> | <span class="strong">**STATUS.C**</span> |
|:----------------------------------------|:-------------------------------------------------------------------------|:----------------------------------------:|
| Values at start:                        | b'01110011'                                                              |                    0                     |
| Rotate Left                             | b'11100110'                                                              |                    0                     |
| Rotate Left again                       | b'11001100'                                                              |                    1                     |
| Rotate Left third time                  | b'10011001'                                                              |                    1                     |

</div>

As you may notice the STATUS.C bit added a 0 to the rotation. So this
will take 9 shifts left to get back to the original value.

<span class="strong">**Simple option**</span>

Many times you want to rotate the variable around like the STATUS.C bit
wasn’t there so the MSB of the variable fills the LSB of the variable on
Rotate Left or the LSB fills the MSB on Rotate Right. That is where the
SIMPLE option comes in. It adds a hidden step that shifts the STATUS.C
bit twice so the bit moves from one end of the variable to the other.

<div class="informaltable">

| <span class="strong">**Command**</span> | <span class="strong">**<span class="emphasis">*variable*</span>**</span> | <span class="strong">**STATUS.C**</span> |
|:----------------------------------------|:-------------------------------------------------------------------------|:----------------------------------------:|
| Values at start:                        | b'01110011'                                                              |                    0                     |
| Rotate Left                             | b'11100110'                                                              |                    0                     |
| Rotate Left again                       | b'11001101'                                                              |                    1                     |
| Rotate Left third time                  | b'10011011'                                                              |                    1                     |

</div>

Notes: The carry is also called SREG bit C, or simply C flag on AVR.

In some cases the Status.C or C flag may already be set because of prior
operations in your program. Therefore, it may be necessary to clear the
C flag before using `Rotate`. Use `Set C Off` before using the `Rotate`
command to clear the flag.

<span class="strong">**Example:**</span>

``` screen
    'This program will use Rotate to show a chasing LED.
    '8 LEDs should be connected to PORTB, one on each pin.

    #chip 16F819, 8

    'Set port direction
    Dir PORTB Out

    'Set initial state of port (bits 0 and 4 on)
    PORTB = b'00010001'

    'Chase
    C = 0
    Do
        Rotate PORTB Right Simple
        Wait 250 ms
    Loop
```

</div>
