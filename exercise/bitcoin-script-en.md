Bitcoin HashLock Script Exercise 
================================
The purpose of this code challenge is to demonstrate understanding of the bitcoin scripting language, 
and usage of a library to construct a script.  

Unless otherwise specified when receiving this excercise, 
any language can be used to solve the problem and any libraries may be used.  

The Exercise
------------
Write a small piece of code to generate a P2SH script and address 
and a piece of code to spend BTC that was send to that P2SH address.  

The script should be a so-called "hashlock script",
one that requires a signature of a private key 
and a secret being revealed when spending (so the redeem script should specify the hash of the secret).

The secret should just be a random value and (manually) shared between the generate and the spending script, 
for the intent of the exercise it's OK to hardcode it as well though.

What is expected of you
-----------------------
Two scripts that can be ran (or one which takes an argument to specify which part to run is fine as well ofcourse);
 - one that generates the P2SH script and address. 
 - and one that spends BTC that was send to it.
 
The step to send BTC to the generated P2SH script can be done manually through bitcoin-cli.

Bonus points for writing some of the code in a way that it could easily be ported from this PoC to an actual codebase for usage.

What if it's too hard
---------------------
We realize this exercise requires strong understanding of the scripting language, 
if it's too complicated then a submission that only does the hashlock and no signing being done (secret hash in redeem script + secret being revealed when spending) 
would be acceptable as well and we can discuss how to add the signing part.  
But ofcourse a complete solution is prefered!
