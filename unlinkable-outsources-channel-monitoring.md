Unlinkable Outsourced Channel Monitoring
========================================

Problem
-------

Many users prefer to user light clients and we can't always trust that they'll be online.
I need to be aways online unlike in Bitcoin.
I also want some source of true blockchain data without 1/2 parties in the channel having to run a full node.
*When you receive coins you want to know as soon as you receive them so that you can grab money back from an invalid close.*

Simple Approaches and Why They Fail
-----------------------------------
Trust a friend to monitor all transaction in the channel or reward them for stopping an illegal close.
But... we want them to stop invalid closes so the reward is meaningless and know they know all of your transaction history.


Approach
--------
Want data to be private and easily stored: 
    - encrypt with a encryption scheme like Schnorr or Mast to combine signatures (but doesn't exist in Bitcoin or Ethereum)
    - if the data is private, might as well let _everyone_ have it, so we have more monitors

Why spread the data: other nodes in the network may go offline as well, so 1 is not good enough.




