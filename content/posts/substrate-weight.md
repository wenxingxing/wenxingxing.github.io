---
title: "Substrate Weight"
date: 2022-11-12T14:55:04+08:00
draft: false
---

## blockchain has limited resource

- limited window per producer
- limited amount of data per block: `MaximumBlockLength`
- given `MaximumBlockWeight` and weight of extrinsic in the tx pool, we select the set of extrinsic to saturate the block, while not exceeding the limit

weight system for developer to tell the block production process how **heavy** an extrinsic is
e
## Fees

```
total_fee = base_fee + length_fee + weight_fee
```
