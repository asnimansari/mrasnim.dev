+++
title = "Fermat's Little Theorm"
date = 2024-06-11
type = "til"
description = "Fermat's Little Theorm"
in_search_index = true
[taxonomies]
tags = ["mathematics", "number theory", "2024"]
+++

Fermat's little theorem states that that if `p` is a `prime number` then for any integer `a`, the number <b>a<sup>p</sup> - a</b> is an integer multiple of `p`.

Which in modular arthemetic transalates to.

a<sup>p</sup> ≡ a (mod p)

#### Example.

let `a = 2` and `p = 7`

a<sup>p</sup> = 2<sup>7</sup> = 128

then `128 - 2` is an integer multiple of `7` which is `7 * 18` or 2<sup>7</sup> ≡ 2 (mod 7)

If `a` is not divisible by `p`, (ie when `a` and `p` are coprime), then a<sup>p-1</sup> ≡ 1 (mod p)

This can we rewritten to find the inverse of a to like a<sup>p-2</sup>a ≡ 1 (mod p). now a<sup>p-2 is the inverse of `a` in `p` prime field.
