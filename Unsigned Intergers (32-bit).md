# Unsigned Intergers (32-bit)

A 32-bit unsigned integer is a data type used in computing to store non-negative whole numbers (zero or positive values).

### Key Characteristics

- **Unsigned:** No sign bit is used, allowing the entire range to represent only non-negative values.
- **Size:** Uses 32 bits (4 bytes) of memory storage.
- **Efficiency:** Provides twice the positive range compared to signed integers of the same bit length.

### Range of Values

A 32-bit unsigned integer can represent the following range:

- **Minimum Value:** 0
    - Binary: `00000000 00000000 00000000 00000000`
- **Maximum Value:** 2³²−1 (4,294,967,295)
    - Binary: `11111111 11111111 11111111 11111111`

### Common Applications

Unsigned integers are primarily used for values that are inherently non-negative:

- Array indices and collection sizes
- Memory addresses and offsets
- Network identifiers (IP addresses, ports)
- Color representations (RGB values, hex codes)
- Resource IDs and counters