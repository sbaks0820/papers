Lightning Network
=================

Requirements
------------
* `SIGHASH_NOINPUT` opcode included in Segwit soft fork that allows spends to be created on transactions that haven't been signed yet.


Approach
---------
Create a funding transaction that is a 2-of-2 multisignature trasnactions but **don't** sign it until Alice and Bob have both created child spend transactions that refund the money back to them.
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

**Necessary Property**: Each party can only broadcast the latest state of the channel otherwise incur a penalty or lose deposit. Need to: blame someone for violation, need to acct on the blame to penalize.

### Naive (Broken) Approach - No Ability to Ascribe Blame
When both parties change the state of the channel (update the Commitment Transaction, CT) they both sign the transaction first and once the Funding Transaction, FT, is in the blockchain the CT can be broadcast.
When a user wants to update the channel, though, there are now two valid spends of FT and we have a race to include in the blockchain: funds stolen/incorrect state recorded/channel closed.

- [ ] Ascribe blame to broadcaster of old state.
- [ ] Penalize broadcaster of old state.

**Note**: _Fidelity Bond_: a form of insurance protection that covers policyholders for losses that they incure as a result of fraudulent acts be specified individuals.

### Naive (Broken) Approach - No Ability to Punish after Blame
You only know who sent out an old transaction if each of Alice and Bobe have a uniquely identifiable Commitment Transaction.
Instead of one CT, there are two CTs one for each of Alice and Bob (they both spend FT but have *different* contents).
Bob signs one CT and sends it to Alice. Alice signs another and sends it to Bob.
Bob has CT1 signed by Alice and Alice has CT2 signed by Bob: either party can now sign the version of the CT they have and broadcast it to the network.
You can distinguish between CT1 and CT2 so we can assign blame for every CT broadcast to the blockchain.
<pre>
                                           +------------------------+
                             +-----------> | Alice = 0.5, Bob = 0.5 | (CT1)
+---------------------+      |             +------------------------+
| Funding Transaction | ---- |
+---------------------+      |             +------------------------+
                             +-----------> | Alice = 0.4, Bob = 0.6 | (CT2)
                                           +------------------------+
</pre>             

- [x] Ascribe blame to broadcaster of old state.
- [ ] Penalize broadcaster of old state.


But... still not way to penalize.
