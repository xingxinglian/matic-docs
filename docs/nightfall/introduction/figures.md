# Differences

- Decentralization (payments and multisig)
- Circuits Transactions
- Poseidon hash to commitment and nullifier
- Secretes encryption


https://github.com/EYBlockchain/nightfall_3/pull/859

This PR closes #733 and allows payments to multiple proposers to be done. Payments are done with ETH for deposits and MATIC for transfers / withdrawals. The transaction structure has been considerably modified to support payments. Each transaction now has two extra nullifiers and one commitment (change) for the fees. In addition, the historical root for the fee nullifiers are added and the fee is also stored in the transaction structure.

Circuits
Public transaction has been modified to accomodate the new public transaction structure. To make sure that the fee nullifiers / commitment are valid, we use the generic verify functions implemented in #843

Operations	Constraints before	Constrains after	% Change
Deposit	921	923	0.2%
Transfer	39,363	64,191	63 %
Withdraw	26,050	50,244	92.8 %
Note: Since deposit is directly paid in ETH, there is almost no change in the number of constraints of the circuit

Smart Contracts

Two changes has been done to the Smart Contract: add the matic address in the config and allow proposers to collect the fees.

Instead of hardcoding the matic address in the circuits, which would imply that for each environment (mainnet, testnet, development...) the address would need to be changed, the matic address is initialized in the smart contracts. Then, if a transaction fee isn't paid in MATIC, that transaction will be challenged.

A feebook mapping (bytes32 -> uint256[2]) has being added in the State.sol to keep track of all the fees paid to the proposer. The input bytes32 corresponds to the keccak of the block proposer and the block L2 number. The first element of the output array corresponds to ETH payments, while the second one corresponds to MATIC payments. The feebook is set when proposing a new block using proposeBlock, and can be claimed using requestBlockPayment.

** NF3 **

The submitTransaction function has been adjusted to perform correctly with the payments.Q




This fixes EYBlockchain/nightfall_3_private#137. It replaces the SHA256 hash, used for commitment and nullifier calculation, with a Poseidon hash. As a result, there are significant circuit changes because most input variables change type to a field. This results in considerable simplification overall however, and much smaller circuits.

Note that this PR is intended to merge into #781 (KEM-DEM) and NOT master


KEMDEM
https://github.com/EYBlockchain/nightfall_3/pull/781
This PR changes the way we encrypt on-chain recipient secrets. The main reasons behind this change are: simplification of the encryption/decryption process, and a reduction in the constraints and on-chain gas costs. Most of these gas costs are gained through the reduction in the amount of data that needs to be sent on-chain as well as the re-use of existing fields within the Transaction struct.

This PR closes #148, #135 and duplicates #69

Results
On-chain Gas Costs
Note: while this directly impacts the transfer operations, the Transaction struct size is reduced by ~25% which indirectly reduces the cost of any operations that use the transactions as CALLDATA.

Operation	Gas Before	Gas After	% Reduction
Deposit	8,636	7,776	-10%
Single Transfer	11,841	9,829	- 17%
Double Transfer	12,602	10,598	-16%
Withdraw	8,902	8,042	-10%
Finalise Withdraw	119,995	117,711	-2%
Circuit Constraints (UPDATED FOR POSEIDON)
Operation	Constraints Before	Constraints After	Poseidon Hash	% Reduction
Deposit	84,766	84,766	619	-99%
Single Transfer	278,682	241,309	41,936	-85%
Double Transfer	499,119	461,746	70,527	-86%
Withdraw	168,883	168,883	27,691	-84%
Testing
This PR can be tested using the standard integration tests.
There are unit tests (property-based) that can be run using npx mocha --timeout 0 --bail --exit test/kem-dem.test.mjs from the root. These currently tests two properties:
(decrypt . encrypt) = id - applying decrypt after encrypt gives the original inputs
(packSecrets . reverse . packSecrets) = id - calling second packSecret on the reverse output of the first packSecret should give the original inputs to the first packSecret.
Protocol Update
Greater details are captured in the updated docs, but a summary is provided here

1) Preparing Inputs
Since Mimc operates on field elements, we move bits from tokenID to ercAddress. It is easier to move 32bits in the circuit.
ercAddress: 160 bits --- top 32 bits from tokenID will be moved here
tokenID: 256 bits --- top 32 bits moved to ercAddress
value: 224 bits
salt: 254 bits

2) Ephemeral Key Generation
x_e <- {0,1}^256
Q_e := x_e.G

3) KEM - Encrypt
shared_secret := x_e . Q_r .... where Q_r is the recipient's public key
encryption_key := mimc(DOMAIN_KEM, shared_secret, Q_e) .... where DOMAIN_KEM := to_field(SHA256("nightfall-kem"))

4) DEM - Encrypt
c_i := mimc(DOMAIN_DEM, encryption_key + i) + p_i ... where c_i and p_i are the ith ciphertext and plaintext respectively

5) KEM - Decrypt
shared_secret := x_r . Q_e .... where x_r is the recipient's private key
encryption_key := mimc(DOMAIN_KEM, shared_secret, Q_e) .... where DOMAIN_DEM := to_field(SHA256("nightfall-dem"))

6) DEM - Decrypt
p_i := c_i - mimc(DOMAIN_DEM, encryption_key + i)



MultiSig
https://github.com/EYBlockchain/nightfall_3/pull/780

This fixes issue D.

The admin application that runs in the administrator container has been heavily modified so that it now works with a multisig contract and onlyOwner actions now require a number of approvals before they can be executed. Full details and information on testing are contained in the nightfall-administrator README.

You will need to build the administrator container if you don't have it.

Note that inclusion of the multisig slightly breaks contract upgradability because a multisig is not compatible with the truffle upgrade plugin. However a simple work around is to disable the multisig temporarily and to use an ephemeral key pair for the upgrade before switching back to the multisig. This can be achieved by calling the relevant contract's transferOwnership function using the multisig.
