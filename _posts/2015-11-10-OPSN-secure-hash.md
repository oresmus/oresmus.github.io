---
layout: post
title:  "moving to more secure hash functions"
listed: false
date:   2015-11-10 17:32:39
author: "Bruce Smith"
part_of_EMSC: true
---

### a hash function's being "cryptographic" is relative to technology and resources ...

For a hash function to be considered "cryptographic",
it must be infeasible for anyone to create new data with a given hash value,
even given existing data with the same hash value.
In fact, it should be infeasible to create *any* different chunks of data with equal hash values
(even though it's obvious that many such "colliding pairs" exist,
since the hash value has a fixed size, shorter than most chunks of data).

This definition is only meaningful relative to the resources, technology, and level of mathematical theory of a society
whose members might try to break the hash function (by finding a "colliding pair",
or worse, a feasible algorithm to alter data without changing its hash value).
That means a given hash function can *lose* its cryptographic status
when the amount or kind of available computing power grows a great deal,
or (as happened with md5) due to theoretical advances (which are unpredictable).

In practice, people depending on a cryptographic hash function try to guess how soon it might be broken,
even though much of the work that might go towards breaking it is secret.
If they are being conservative, they'll want to replace it as soon as its breakage looks the least bit possible
(which is long before it seems actually likely). Others might value convenience
(of using fast or standard functions, or those with shorter hash values)
over absolute safety -- they might even continue to use a function for which a colliding pair has been found,
arguing (correctly) that it's much better than nothing and that this doesn't immediately lead to each use of it
being forgeable. Of course there is a spectrum of opinions along this continuum. The currently most popular
cryptographic hash function is sha1, which appears far from being broken,
but some people already advocate moving to newer and longer-valued hash functions for various reasons.
(There are always candidates to move to -- both wholly new algorithms,
and systematic ways of differently using and/or combining
existing hash functions to make something more secure
(for example, concatenating the output of unrelated hash functions applied to the same data).)

### ... so we'd better be able to replace it with a better one.

To summarize, **it's inevitable that any given hash function will one day need replacement,**
but people have different opinions about when to do that, and what function to replace it with.
So a system like OPSN,
which depends on reliable hash-based references,
but intends **to support a large and long-lasting decentralized network**
(of similar size and duration as the societies making use of it),
needs to allow references using different hash functions to coexist,
and **must permit a gradual changeover** driven by individual decisions
about which hash functions to use or accept in references.
It's also desirable if there are ways for interested subcommunities
to use more secure hashes for critical data,
and to detect and respond to a mostly-secret hash function breakage
(by detecting attempts to use it to forge specific references).

### Fortunately, we can.

In any normal situation, a hash function changeover will happen gradually,
and new hash functions will be well-known
under standard names (and available in client code for use in references) by the time they're needed.

But even if it happened suddenly
and the new functions had nonstandardized names or unsettled algorithms,
everything could still work fine for a subcommunity making the change
(once their client code implemented the same version of the same hash function
under the same name or a generally-known set of name-variants),
with the worst case being a temporary "network partition" in which the other side's references
were deemed invalid
(plus permanent uncertainty about the true defining data of symbols or items
which in hindsight could have been faked, since for awhile they only existed on the unsafe side).

The reasons this is the worst case (if the users of OPSN are making sensible preparations for this) are:

* if you're not sure which hash algorithm was used to make a given reference,
out of any reasonable number of possibilities (for interpreting the nickname it gives for its hash function)
which you trust as being cryptographic, you can just try them all --
whichever one makes it seem like a valid reference
(since you can look up properly formatted defining data which hashes to the right value)
is the right one.

* you can make references which contain hash values from more than one hash function (see "in-reference metainfo" elsewhere),
and make rules for what to do when some of them are broken individually (but not when used in that combination).

* if you know that both a reference, and some data it appears to point to,
are old enough that they
were made when (you believe) the involved hash function was still cryptographic,
then you can still trust the reference as referring to that data
(since when they were made, there was no way to make such a pair
except hashing that data to get the unique hash value in the reference).
You can know those things if the data and reference were digitally notarized (with a hash function you still trust).
Routine use of OPSN will continuously digitally notarize everything in it
(as a side effect of propagating hashes in new references);
we can maintain a parallel, slower distribution channel
which does this in a batched way, computing fewer but better hashes,
so that channel can use more expensive hash functions which will remain cryptographic longer.

* an entire graph can be replaced with one using a different hash function in an efficient distributed way,
using an obvious algorithm which only requires loose coordination, which basically consists of the formal statement
"please tell me when you have a version of this reference which uses this hash function
(not only in itself but in everything it refers to, even indirectly)".
(Note that the graph of hashes can't contain cycles
(unless it *already* contains forged references using broken hashes),
as discussed elsewhere.)

### (We can also detect and prepare for the problem.)

We mentioned above some desirable extra features; here are ways to implement them:

* there should be a way for an interested subcommunity to
**use more secure hashes for critical data** -- they can just concatenate several hashes,
or use a hash on preprocessed data, to make more secure hashes for this, and use them in critical references.

* there should be a way to
**detect and respond to a mostly-secret hash function breakage**
(by detecting attempts to use it to forge specific references). One way to detect this is for a subcommunity to make a practice
of adding extra hashes to some references they pass around (or to distribute them separately),
using new variant hashes each time a reference is copied and resent
(for example, by hashing a varying prefix plus the input data). Then when following any reference using the standard hash,
they also verify the extra hash.
To respond to a detected collision, they spread alerts with the details,
using some of the "in-reference metainfo" described elsewhere.
(But when writing code for handling that response, see the cautions in
["**avoiding cascading failure**"]({% post_url 2015-11-10-OPSN-cascading-failure %}).)


