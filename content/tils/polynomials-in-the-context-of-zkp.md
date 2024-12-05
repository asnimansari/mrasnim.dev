+++
title = "Polynomials In The Context Of ZKP"
date = 2024-06-10
type = "til"
description = "Polynomials In The Context Of ZKP"
in_search_index = true
[taxonomies]
tags = ["ZKP", "mathematics"]
+++

Why polynomials are relevant in ZKP..

1. They can store unbounded amount of data.
2. They can store relationship between data.
3. They help to check if two pieces of data are same efficiently.

### Encoding Unbounded amount of data.

Polynomials can store data in the form of coeffients and exponents..

Example.</br>
The binary number `10011` can be encoded as

<div align='center' style='font-size: 24px;'><b>x<sup>4</sup>+x<sup>1</sup>+1</b></div>

Another binary number say `11100000000000101010111` could be encoded as

<div align='center' style='font-size: 24px;'><b>x<sup>19</sup>+x<sup>17</sup>+x<sup>16</sup>+x<sup>10</sup>+x<sup>5</sup>+x<sup>3</sup>+x<sup>2</sup>+x<sup>1</sup>+1</b></div>

This is a polynomial of degree 19.

Once we have these polynomials, we can then recover the data by evaluating the polynomial at specific points.
here, by evaluating the polynomial at `x = 2` we can recover the number.

### Encoding relationship between data.

Along with storing data, polynomials can also store <b>relationship</b> between data. This process is called <b>polynomial interpolation</b>.

To find the polynomial that passes through a set of points, we can use the <b>Lagrange interpolation</b> method.
![image](/images/til/lagrange-interpolation.png)

### Checking if two pieces of data are same.

Comparing two polynomials is a hard problem. We could use brute force to check if two polynomials are same by checking if they are equal at all points.

But this is inefficient.
The other approach is [Schwartz-Zippel lemma](https://en.wikipedia.org/wiki/Schwartz%E2%80%93Zippel_lemma), which states that the probability of a nonzero polynomial evaluating to zero at a random point is, at most, the degree of the polynomial divided by the number of possible evaluation points.

This is not the perfect solution, but it is a good enough solution for most cases and ZKP
