---
categories:
  - Computer Architecture
tags: [binary, binary-encoding]
---

# Signed and unsigned numbers

In order to represent negative integers in binary we use signed numbers. **Signed binary** is basically binary where negative integers can be represented. **Unsigned binary** is standard binary without negative integers.

## Two's complement

Signed numbers can be implemented in binary in a number of ways. The differences come down to how you choose to encode the negative integers. A common method is to use "two's complement".

> The two's complement of a given binary number is its negative equivalent

For example the two's complement of $0101$ (binary 5) is $1011$. There is a simple algorithm at work to generate the complement for 4-bit number:

1. Take the unsigned number, and flip the bits. In other words: invert the values, so $0$ becomes $1$ and $1$ becomes $0$.
2. Add one

![](/img/unsigned-to-signed.png)

To translate a signed number to an unsigned number you flip them back and still add one:

![](/img/signed-to-unsigned.png)

### Advantages

The chief advantage of the two's complement technique of signing numbers is that its circuit implementation is no different from the adding of two unsigned numbers. Once the signing algorithm is applied the addition can be passed through an [adder](/Electronics_and_Hardware/Digital_circuits/Half_adder_and_full_adder.md) component without any special handling or additional hardware.

Let's demonstrate this with the following addition:

$$
    7 + -3 = 4
$$

First we convert $7$ to binary: $0111$.

Then we convert $-3$ to unsigned binary, by converting $3$ to its two's complement

$$
0011 \rArr 1100 \rArr 1101
$$

Then we simply add the binary numbers regardless of whether they happen to be positive or negative integers in decimal:

$$
0111 \\
1101 \\
====\\
0100
$$

Which is 4. This means the calculation above would be identical whether we were calculating $7 + -3$ or $7 + 13$.

The ease by which we conduct signed arithmetic with standard hardware contrasts with alternative approaches to signing numbers. An example of another approach is **signed magnitude representation**. A basic implemetation of this would be to say that for a given bit-length (6, 16, 32...) if the [most significant bit](/Electronics_and_Hardware/Digital_circuits/Half_adder_and_full_adder.md#binary-arithmetic) is a 0 then the number is positive. If it is 1 then it is negative.

This works but it requires extra complexity to in a system's design to account for the bit that has a special meaning. Adder components would need to be modified to account for it.

## Shorthand for deriving two's complement

A simple way to work out the value of a signed number as contrasted with an unsigned number is to schematize it as follows: _the most significant place has a weight equal to the negative value of that place, and all other places have weights equal to the positive values of those places_.

Thus for a 4-bit number:

Then if we add the decimal equivalents of the place value together, we get our signed number. So in the case of $-3$:

![](/img/signed-conversion.png)

## Considerations

A limitation of signed numbers via two's complement is that it reduces the total informational capacity of a 4-bit number. Instead 16 permutations of bits giving you sixteen integers you instead have 8 integers and 8 of their negative equivalents. So if you wanted to represent integers greater than decimal 8 you would need to increase the bit length.