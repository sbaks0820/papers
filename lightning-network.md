Lightning Network
=================

Requirements
------------
- `SIGHASH_NOINPUT` opcode included in Segwit soft fork that allows spends to be created on transactions that haven't been signed yet.


Approach
---------
Create a funding transaction that is a 2-of-2 multisignature trasnactions but *don't* sign it until Alice and Bob have both created child spend transactions that refund the money back to them.
The `SIGHASH_NOINPUT` opcode lets you do this since signature is not part of txid anymore.

Order of Operations:
1. Create the parent (Funding Transaction)
2. Create the children (Commitment Transactions and all spends from the commitement transactions).
3. Sign the children.
4. Exchange the signatures for the children.
5. Sign the parent.
6. Exchange the signatures for the parent.
7. Broadcast the parent on the blockchains.

Commitment transactions track the current balance of the channel since they are not broadcast to the network. With one commitment transaction exchanged, Alice and Bob can both get their refund.
Only broadcast commitment transactions when you want to close the channel.
