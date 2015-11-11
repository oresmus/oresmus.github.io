---
layout: post
title:  "garbage collection"
listed: false
date:   2015-11-10 17:32:15
author: "Bruce Smith"
part_of_EMSC: true
---


### basic algorithm

In the purest form of OPSN, each user's server
subscribes to all data they might be interested in,
and keeps forever (and possibly reshares) everything it receives.

For small data (like manually written text) shared within small communities,
this might be practical,
but in general it's sometimes necessary for a user to discard data.
In fact, for some kinds of shared data -- like moment-to-moment state in an online game,
or transient details of collaborative text editing (like exact timing of typed characters and complete undo history) --
most users want to discard most of it fairly soon after receiving or generating it.

Each user has full control in principle over what data to keep and for how long --
they have a *right* to keep and republish data they receive, not a *responsibility* --
but it's important to understand what strategy they might use in practice
to make the problem manageable and avoid losing something they wanted to keep.

The basic strategy is captured by "each user keeps (and can reshare, then or later) the subset they're interested in".
Obviously this must be weighted by the cost of keeping the data
(which differs greatly between types, due to storage size),
and it requires the system to have a good estimate of how much the user cares about each item
(or about each small "batch" of items, to be kept or discarded together).
It might also be useful to take into account how soon the user might next want the data (as estimated from links to it),
and whether other users (or data-storage services) also have it.

Estimating the user's interest is a hard problem, but it's exactly the problem OPSN needs to solve anyway
to know what data to *show* the user -- it just has a much higher threshold for what to show them
than for what to keep. (Another difference is that the moment-to-moment question about what to show the user
depends on the state of the user's view, including what other items, topics, or relations they're browsing right then.)

It is also a problem OPSN is designed to help the user solve, as detailed elsewhere.

(There is also an issue of items needed "indirectly", e.g. items containing code to help the user view other items,
which are not something the user thinks of as "interesting" except due to that use (which they might not even be aware of).
For these, the user depends on their system to propagate interest along certain kinds of links.
For example, if the system plans to run some code C1 to help make use of interesting item I,
and if code C1 refers to code C2 (using a reference that is not marked "optional" in some manner),
the system ought to consider C1 and C2 as being at least as necessary to keep as I is.
This general problem in a software system (or more precisely its inverse, to find things *not* wanted)
is called "garbage collection",
or "distributed garbage collection" if the links requiring propagation of interest can point to objects in remote systems.
To a first approximation, OPSN can handle it by copying remote link targets to make them local,
and then using non-distributed garbage collection algorithms, which can be complex but are well-known.
We get a further big simplification by focusing mainly on immutable objects (since for them the algorithm can be trivial).
Exceptions to this are mentioned below.)

So we assume the system is able to estimate the value of keeping each item.
Of course, the user can also set preferences and create rules specifically meant to affect this --
most simply, they can mark some items to "always keep" (perhaps automatically keep-marking all small items they read).
They can also keep items for the sake of future users who might find them,
even if they no longer want to view them themselves.
And they can examine the results of this computed estimate at any time --
for example, a user wanting to tune these settings
could browse the items on the borderline between keep vs discard,
or scan a random sample of items about to be discarded.

Each user's system then just keeps the items above some threshold of estimated value vs. cost to keep, and discards the rest.
(In principle it does this continuously, though it ought to assume that recent changes in keep-related preferences
might be user errors, and therefore not be too quick to discard things after such changes;
in practice it would typically optimize by doing cleanups only periodically.)

### so items are kept as long as anyone wants to keep them

The result of this, for the community as a whole,
is that a given item will be kept by each user who thinks it's worth keeping,
considering just that user's interest and cost in keeping it
(though their interest in keeping it can depend on their estimate of future interest by others, if they like).

And, whenever some user keeps an item (in the usual public way), it can be found, and copied or reshared,
by other users searching for it or items like it. (Search services haven't been discussed specifically here,
but whoever maintained them would keep references to all indexed items, from which they could be found.)

Thus no item will disappear completely unless (at any particular time)
*no user* thinks it's worth their own cost of keeping it.

### sharing the cost?

But what about items that many users want *someone* to keep,
but no one wants to pay the full cost of keeping?
(For example, large items that many users think have long-run value but only want to examine rarely,
or large collections that many users think have high value as a complete collection,
but at any given time each user only wants to make use of a few items in it.)

For these there is a coordination problem,
since collectively the users think the items are worth *someone* keeping,
but they need a way to share the cost among the users who think that,
not just among the few users (perhaps chosen arbitrarily) who actually keep them.

There are several possible solutions to this -- in fact several which are obvious and presumably well-known:

* a company can store such items, and charge each interested user a fraction of the cost,
for the right to view or copy (and reshare) the item at any future time
(with cost to be revised periodically).

* users can trade such services, formally or informally, with other users interested in similar items.

The OPSN protocol makes solutions like this easier,
due to each item having a unique address (its uid)
(so it can be found, and remains connected to the same other items, regardless of where it's stored),
and since the protocol can be extended (by defining symbols with appropriate meanings)
to support formal statements which mean things like
"last call for copying this item which I no longer want, and plan to discard in 2 weeks",
or "please tell me (using a suitable formal statement) if you are about to discard this item you have
(which I think someone ought to keep)".

(I got the idea for that set of formal statements
from a comment by Chip Morningstar,
describing the work of a colleague
on distributed garbage collection (probably Arturo Bejar at Electric Communities).
I don't know whether I interpreted Chip's comment correctly.)

