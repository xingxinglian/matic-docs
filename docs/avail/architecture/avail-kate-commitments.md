---
id: avail-kate
title: Kate Commitments on Avail
sidebar_label: Kate Commitments
description: Learn how to use Polygon Avail
keywords:
  - docs
  - polygon
  - avail
  - data
  - kate
  - commits
  - commitments
image: https://matic.network/banners/matic-network-16x9.png 
slug: avail-kate
---

## Secure mathematical primitives

Blockchains change states based on the status and activity revolving 
transaction data, utilize data structures to link data together, and 
verify the related data.

Cryptography and its associated mathematical primitives are at the core 
of blockchain technology. A few mathematical components come together to 
offer secure communication in the form of cryptography. 

One of those components is commitments. Commitment schemes are a key 
mathematical primitive in cryptography that allows a user to commit
a value (or statement) without revealing the value, but are
able to reveal that value at a later time.

Merkle trees are a commonly used type of data structure in blockchains 
and can be used as a cryptographic commitment scheme. They are considered 
to be a specific type of commitment known as a *vector* commitment. Merkle 
trees store the state of the blockchain, where the [Merkle] root is the
hash of all hashes within the tree, meaning that committing the root is 
essentially committing all the states for that Merkle tree, 

## Why do we need polynomial commitment schemes?


## What is the difference between vector and polynomial commitments?


## What are Kate commitments?


## What are the benefits of using Kate commitments?

Kate commitments allow a node to commit to values succinctly to be kept 
inside the block header. Short openings are possible, which helps a light 
client verify availability. The cryptographic binding property helps us 
avoid fraud proofs by making it computationally infeasible to produce 
wrong commitments.
