---
id: zk-proofs
title: ZK Proofs
sidebar_label: ZK Proofs
description: ZK Proofs
keywords:
  - docs
  - polygon
  - nightfall
  - zk
  - proof
image: https://matic.network/banners/matic-network-16x9.png
---

User transactions are always verified by an Ethereum smart contract by verifying the ZK-Proof supplied by the user. The specific ZK-SNARK that is used in these ZK-Proofs is [`Groth16`](https://eprint.iacr.org/2016/260.pdf). This protocol has been widely used and tested by the Zcash team of researchers and it is currently considered mature enough to be used in production.

At this time, Ethereum precompiled smart contracts only support BN254 elliptic curve operations for zk-SNARK proofs validation. For this reason, Nightfall uses this curve for generating and validating proofs for implementing elliptic curve cryptography inside circuits.

### Multi-party Computation for the Trusted Setup

The proving and verification keys of the ZK-SNARK protocol require the generation of some random values that need to be eliminated. This elimination process is a crucial step: if these values are ever exposed, the security of the whole scheme is compromised. To construct the setting, Nightfall uses a [`Multi-party computation (MPC)`](https://en.wikipedia.org/wiki/Secure_multi-party_computation) ceremony that allows multiple independent parties to collaboratively construct the parameters or trusted setup. With MPC, it is enough that one single participant deletes its secret counterpart of the contribution in order to keep the whole scheme secure.

The construction of the trusted setup has two phases:
1. General MPC ceremony that is valid for any circuit (also known as Powers of Tau ceremony)
2. Phase 2 that is constructed for each specific circuit.

Anyone can contribute with their randomness to the MPC ceremonies and typically, before getting the final parameters, a random beacon is applied.

#### Radix files

The radix files are the MPC params that are calculated from the latest response file from PPOT. These radix files have specific depths depending on the number of constraints for each circuit, and can take a while to compute. They have been stored in a publicly accessible S3 bucket (https://nightfallv3-proving-files.s3.eu-west-1.amazonaws.com/radix).

You can start the process on your own and create your own radix files. You don't need to trust anybody. For that, you need to get the latest response from the [PPOT ceremony](https://github.com/weijiekoh/perpetualpowersoftau) and download the `new_challenge` file, rename it to `challenge` and run the `mpc.sh` script you'll find [here](https://github.com/EYBlockchain/nightfall_3/blob/master/zokrates-worker/src/mpc.sh). This script will spit out a bunch of radix files like `phase1radix2m2`, `phase1radix2m3`... `phase1radix2m(n)` where `n` should be the depth of the circuit we're using.

This means number of constraints, which you can get by running `zokrates compile` to compile the circuits, and then `zokrates inspect` on the compiled circuits, which should spit out the number of constraints. You should pick a value of `n` for which `2^n` is bigger than the number of constraints. For example, at the time of writing, the compiled `deposit` circuit has `84766` constraints so we need to pick up the `phase1radix2m17` as `2^16 < 84766 < 2^17`.

You should rename these radix files to the name of the circuits, and host them somewhere. IPFS, S3 buckets, your own webserver, whatever you want.

Lastly, don't forget to modify the env variable `RADIX_FILES_URL` to point to the URL where you'll get the radix files.

## Cryptography

MiMC
