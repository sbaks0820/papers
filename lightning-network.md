Lightning Network
=================

Requirements
------------
* `SIGHASH_NOINPUT` opcode included in Segwit soft fork that allows spends to be created on transactions that haven't been signed yet.


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


### Naive (Broken) Approach
When both parties change the state of the channel (update the Commitment Transaction, CT) they both sign the transaction first and once the Funding Transaction, FT, is in the blockchain the CT can be broadcast.
When a user wants to update the channel, though, there are now two valid spends of FT and we have a race to include in the blockchain: funds stolen/incorrect state recorded/channel closed.

*Note*: _Fidelity Bond_: a form of insurance protection that covers policyholders for losses that they incure as a result of fraudulent acts be specified individuals.


