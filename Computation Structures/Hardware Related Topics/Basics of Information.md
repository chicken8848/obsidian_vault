# Number systems
- Binary: base 2
- Octal: base 8
- Hex: base 16

## 2's Complement

2’s Complement is the way most computers or electronic machines choose to represent _signed_ integers. Given a string of bits, we can compute its negative representation using 2’s Complement.

Most computers use the most significant bit to be negative, keeping all other bits positive.

Steps to compute a negative version of a number:
1. Logical not on all bits
2. Add 1
Steps to compute positive version of a number:
1. minus 1
2. logical not on all bits

## Decimal Encoding

Put a point somewhere in your bit string

```
1001.0011
```

Is the same as is in decimal
$$-2^{3} + 2^{0} +2^{-3} + 2^{-4}$$
# Encoding
Is the process of representing information, other than numbers. There are two types of encoding:
- Fixed length encoding: When all the choices are equally probable
- Variable length encoding: When not all choices are equally probable

![[Pasted image 20240121003922.png]]

Example of encoding is character encoding so that the string of bits can be displayed
- Number Encoding: 4-bits to represent each number 1 to 10
- 7-bit ASCII encoding for english characters
- ISO-8895-1 in ISO-8859-1 encoding is 8 bits
- A unicode character in UTF-8 encoding is between 8 bits to 32 bits
- 16-bit unicode (UTF-16): for other language alphabets that are fixed, e.g Russian
We can create a map given encoded information, to manipulate for ourselves and other devices.

8 bits = 1 byte

# Information and uncertainty
