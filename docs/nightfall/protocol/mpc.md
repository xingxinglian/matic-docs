
# MPC Ceremony
Zero-knowledge proofs require a trusted setup. This setup generates a so-called "toxic waste" which could potentially allow to create fake proofs. To avoid this, a ceremony needs to be held where this setup is generated via multi-party computation (MPC).

Nightfall ran a Multi-Party Computation (MPC) following the same principles of the Perpetual Powers of Tau Ceremony. The process started with contribution 72 from  Perpetual Powers of Tau Ceremony for BN254 Curve. You can find this contribution [here](https://github.com/weijiekoh/perpetualpowersoftau/tree/master/0071_edward_response).

Since Nightfall uses the Groth16 proving scheme, a second phase of the MPC is needed, for each circuit. 4 private contributions were part of this phase2:

1. [Darko Macesic (github ID: dark64)](https://github.com/maticnetwork/nightfall_phase2ceremony/blob/main/atttestations/1_Darko.md)
2. [Jordi Bailyna (github ID: jbaylina)](https://github.com/maticnetwork/nightfall_phase2ceremony/blob/main/atttestations/2_Baylina.md)
3. [Paul Brody (EY Global Blockchain Leader)](https://github.com/maticnetwork/nightfall_phase2ceremony/blob/main/atttestations/3_Brody.md)
4. [Michael Connor (github ID: iAmMichaelConnor)](https://github.com/maticnetwork/nightfall_phase2ceremony/blob/main/atttestations/4_Connor.md)

After the last contribution, we applied a random beacon for the 4 circuits. For this beacon we created a mainnet [transaction](https://etherscan.io/tx/0xd42eff8e34aa9227cdceb12daf1d868b3dec025ac23073cfd103bb697642dbc1) with the data payload 0xe095cb (in decimal this is 14718411).

This transaction was included in block number [14711908](https://etherscan.io/block/14711908), which 
landed on May 5, 2022 at 05:00:27 PM +UTC and had the blockhash
 `0x875966a4d290bae914acd733315d1a1cbea3fb2b9fde133a0c6fffa7f726cbe3`. 
 This hash was then hashed recursively 1024 times. The output is `0x144212c1ae36d729307364dcb845a04b9c5f523fe557eb777a910d4ea6cc5a09`.

The final hash was computed with this program:

```js
import crypto from 'crypto';

let h = '875966a4d290bae914acd733315d1a1cbea3fb2b9fde133a0c6fffa7f726cbe3';
for (let i = 0; i < 1024; i++) {
  h = crypto.createHash('sha256').update(h, 'hex').digest('hex');
}
console.log(h);
```

 The complete information on phase 2 can be found [here](https://github.com/maticnetwork/nightfall_phase2ceremony/blob/main/atttestations/phase2.md).

