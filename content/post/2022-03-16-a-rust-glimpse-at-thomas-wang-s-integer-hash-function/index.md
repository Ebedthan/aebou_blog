---
title: A Rust glimpse at Thomas Wang's integer hash function
author: Anicet Ebou
date: '2022-03-16'
slug: a-rust-glimpse-at-thomas-Wang-integer-hash-function
categories: []
tags: []
menu: main
---

In 1997, [Thomas Wang introduced an integer hash function](http://www.concentric.net/~ttwang/tech/inthash.htm)
using some techniques invented by [Bob Jenkins](http://burtleburtle.net/bob/hash).
The inverted version of this hash was then introduced by [Geoffrey Irving](https://naml.us/post/inverse-of-a-hash-function/).

This hash function found his way to bioinformatics through [Heng Li](https://gist.github.com/lh3/974ced188be2f90422cc) which
used it in miniasm and minimap. 

Pandey et al., further used it also to make the counting quotient filter exact in
[squeakr](https://github.com/splatlab/squeakr).

Beside work from [Jean-Pierre Both](https://github.com/jean-pierreBoth/probminhash),
this function have not been explored in Rust albeit to the best of my knowledge.

Here I provides a simple glimpse at his implementation in Rust.

## Key properties of the int hash

> A good mixing function must be reversible.
>
> Thomas Wang

The key property of Thomas Wang's integer hash function are avalanche and invertibility.

A hash function has form h(x) -> y.  Avalanche means that a single bit of difference in the input will make about 1/2 of the output bits be different.

Trying to implement theses function in Rust was harder to first though especially
to find a non buggy mask version. The below implementation does not use a mask.
(If you find a way to make a version with a mask, please let me know!)

## hash_64(): int hash 64 bit version

```rust
fn hash_64(key: u64) -> u64 {
    let mut h_key = key;

    // key = (key << 21) - key - 1
    h_key = (!h_key).wrapping_add(h_key << 21);
    h_key = h_key ^ h_key >> 24;

    // key * 265
    h_key = h_key.wrapping_add(h_key << 3).wrapping_add(h_key << 8);
    h_key = h_key ^ h_key >> 14;

    // key * 21
    h_key = h_key.wrapping_add(h_key << 2).wrapping_add(h_key << 4);
    h_key = h_key ^ h_key >> 28;

    h_key = h_key.wrapping_add(h_key << 31);

    h_key
}
```

First, I could have took as input a mut key but just assigning key to a mutable
variable in the function seems enough for me. Using the primitive methods [`wrapping_add`](https://doc.rust-lang.org/std/primitive.u64.html#method.wrapping_add) gives a different code aspect from the C one, but helped my eyes a lot by giving operations a chain like structure.


## hash_64i(): invertible int hash 64 bit version

```rust
fn hash_64i(hashed_key: u64) -> u64 {
    let mut key = hashed_key;

    // Invert h_key = h_key.wrapping_add(h_key << 31)
    let mut tmp: u64 = key.wrapping_sub(key << 31);
    key = key.wrapping_sub(tmp << 31);
    
    // Invert h_key = h_key ^ h_key >> 28;
    tmp = key ^ key >> 28;
    key = key ^ tmp >> 28;
    
    // Invert h_key = h_key.wrapping_add(h_key << 2).wrapping_add(h_key << 4)
    key = key.wrapping_mul(14933078535860113213u64);
    
    // Invert h_key = h_key ^ h_key >> 14;
    tmp = key ^ key >> 14;
    tmp = key ^ tmp >> 14;
    tmp = key ^ tmp >> 14;
    key = key ^ tmp >> 14;
    
    // Invert h_key = h_key.wrapping_add(h_key << 3).wrapping_add(h_key << 8)
    key = key.wrapping_mul(15244667743933553977u64);
    
    // Invert h_key = h_key ^ h_key >> 24
    tmp = key ^ key >> 24;
    key = key ^ tmp >> 24;
    
    // Invert h_key = (!h_key).wrapping_add(h_key << 21)
    tmp = !key;
    tmp = !(key.wrapping_sub(tmp << 21));
    tmp = !(key.wrapping_sub(tmp << 21));
    key = !(key.wrapping_sub(tmp << 21));

    key
}
```

Most of the lines are inverted by:
* breaking the key into two,
* leaving one piece alone and running the second piece through an invertible function that depends on the first.

Geoffrey Irving's post is a better place to understand what is going on.

Don't think you have a problem if you don't understand at first waht is going on
in these function, this topic is hard enough and bitewise operations logic don't like
to stay in our head. So do not hesitate to go back to simple terms to have a better
understanding. [Rust operators](https://doc.rust-lang.org/book/appendix-02-operators.html) can help.

Have a nice day!

