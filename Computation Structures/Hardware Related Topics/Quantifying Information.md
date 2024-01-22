# Information and Uncertainty
The amount of information held by an event is inversely proportional to the probability of that event happening
$$ \text{Information} \propto \text{Uncertainty} \propto \frac{1}{p} $$
Or proportional to the the uncertainty that event happening. For discrete events $(x_1, x_2, \dots, x_n)$ with probability of occurrence of $(p_1, p_2, \dots, p_n)$, the basic measure is the bit.

Number of bits required to reveal a random variable
$$ I(x) = \text{log}_2 \frac{1}{p_i} $$
## Narrowing Down Choices
With N equally probable choices, if it is narrowed down to M choices (where N > M), then
$$ I_{N \rightarrow M} (X) = log_2 (\frac{N}{M}) $$

# Error Detection and Correction
Suppose we want to reliably transmit the result of a single coin flip. And then a single bit error happens: "0" is then transmitted as a "1" - a single bit error

## Hamming Distance
The number of digit positions in which the corresponding digits of two encodings of the same length are different. The problem with simple encoding is that the two valid code words ("0" and "1") also have a Hamming distance of 1. So a single-bit error changes a valid code word into another valid code word.

## Error Detection
What we need is an encoding where a single-bit error doesn't produce another valid code word.

We can add single-bit error detection to any length code word by adding a parity bit chosen to guarantee the Hamming distance between any two valid code words is at least 2.

But we still cannot detect errors.

By increasing the Hamming distance between valid code words to 3, we guarantee that the sets of words produced by single-bit errors don't overlap. So if we detect and error we can perform **error correction** since we can tell what the valid code was before the error happened.

# Summary
- Information resolves uncertainty
- Choices equally probable:
	- N choices down to M needs $log_2 \frac{N}{M}$ bits of information
	- use fixed-length encodings
- Choices not equally probable:
	- choice with probability $p_i$ needs $log_2 \frac{1}{p_i}$ bits of information
	- average number of bits = $\sum p_i log_2 \frac{1}{p_i}$
	- use variable-length encodings
- To detect D-bit errors: Hamming distance > D
- To correct D-bit errors: Hamming distance > 2D