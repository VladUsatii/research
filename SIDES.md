# SIDES : Symmetrical Integral Derivative Encryption Standard

#### Written by Vlad Usatii | CEO @ Gem

## Introduction

The first encryption standards used, quite heavily, the idea of one-way trapdoor functions. These functions took some standard input, and for every input, made a clear one-way transition to a padded output (e.g. SHA-1, keccak). The problem with these standards is that checking the validity of any encrypted field is akin to asking someone to generate a long list of random numbers and letters and comparing outputs to the original encrypted field. This is how SHA-1 was "cracked," for example. SHA-3 and the regularly used SHA-256 are popular examples of the use of more alphanumeric indexes as a means of solving the brute-force collision approach. Still, it lacks use in other contexts involving unrecoverable data (keys).

To resolve and create symmetry in the field of encryption, another idea -- the simple idea of derivatives and integrals -- is employed. A few basic things that a calculus viewpoint adds to the idea of encryption is the work behind "guessing" integrable functions via the Taylor Series, and in special cases, the Maclaurin Series. This creates a pseudo-one-way function, such that it would be inefficient to use output as a way to rediscover input values.

This way, it would be computationally cheap to generate a random order derivative (e.g. 4 derivatives), flatten bytecode into their ordinal (integer-associated) values, take the derivative assuming polynomials stand in front of each coefficient, and share the output value. In return, the host who had encrypted the values would hold onto the digits that are multiplied by a 0th-order exponent. The order could be recovered by taking the length operation of the digits that had a 0th-order exponent applied to them.

In practice, storing data on the cloud would be as easy as generating some high-order derivative value (generator), taking the derivative of the flattened bytes, and storing the output in the machine, whilst the host would store a matrix of the "removed" constants. This way, less data would be stored in the destination machine, yet it would still be accessible via the 0th-power constants.

## In practice

Let's say we have the following data ```msg = b'hello'```. We can first transform this data into its ordinal equivalent by doing the following:

```python3
matrix_form = [x for x in msg]
# output: [104, 101, 108, 108, 111]
```

Then, we create a random number ```< len(matrix_form)```. This number is called the order. It tells us how many times we will take the derivative of the coefficients listed in polynomial form.

So, imagine that the matrix/ordinal form looks like this: ```104x**4 + 101x**3 + 108x**2 + 108x**1 + 111x**0```. That is the polynomial form that I am referring to.

Now, we "take the derivative" as many times as our order tells us to (let's say it is ```2```). Our output is calculated by the following operations (```o(n)``` is the n'th order):

```
o(1) = [416, 303, 216, 108] (111)
```

The set ```(111)``` is referring to the constant. This constant is stored with the host.

```
o(2) = [416*3, 303*2, 216*1] (111, 108)
```

We can safely share the matrix ```[416*3, 303*2, 216]``` with our peers, but we hold onto ```(111, 108)```. It is the constants. The length of this set is equal to the order.

Now, if we unflatten the ```o(2)``` output, we receive an untillegible message:

```python3
encrypted_msg = [chr(x) for x in o(2)]
# output: ['Ӡ', 'ɞ', 'Ø']

pt = ""
for x in encrypted_msg:
    pt += x
# output: ӠɞØ
```

You can send ```pt``` (plaintext) to your peers. In return, you are storing unintelligible data on the destination machine, but on your source machine, you have the key to integrate and retrieve your data.

## Problems

The biggest problem with SIDES is by far the idea that there aren't an infinite amount of Unicode-associated ordinal values. If you take the derivative at too high an order, you may realize that the Unicode output doesn't exist, quite obviously because there isn't an infinite amount of Unicode characters. Because of this, a basic fix would be to limit your derivatives. In fact, taking too many derivatives results in wasting a lot of precious data. This is because the numbers stored after taking the derivative ```n``` times may take up more space than the raw message.

------
Last updated on Sun Jun 19 2022
