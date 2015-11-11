---
layout: post
title:  "avoiding cascading failure"
listed: false
date:   2015-11-10 17:32:12
author: "Bruce Smith"
part_of_EMSC: true
---


Any highly connected system has risks of "cascading failure",
where failure of one part causes problems and possible failure in other parts,
which in the worst case can spread to much of the system.
It is worth trying to reduce this risk
and to mitigate its effects.
(In practice it's enough to make this risk
lower for this system than for existing systems we or it depend on,
like food and electricity distribution, and the Internet.
We can hope this bar becomes higher over time.)

A cascading failure could in principle be triggered by localized problems with various causes,
or by network partition,
or even by poorly managed gradual evolution (such as optimizations which reduce reserve capacity)
which eventually lead to a "phase change" in which some disruptions grow rather than damping down.
It can even be exacerbated by systems meant to be protective (as with an autoimmune disease),
like certain responses to an alert spread when a hash collision, bug, or software virus is detected.

The ways to reduce the risk are the same for this system as for any other:

* try to understand it in detail,
perhaps by simulating failures or responses to them
(and use what you learn to improve resilience);

* encourage a diversity of implementations of components
(especially useful to reduce risks from *intentional* cascading failures,
like viruses, which often take advantage of bugs in specific implementations;
but also helps by making all kinds of unanticipated behaviors more diverse);

* treat information about exactly which implementation a network node uses as private
(to make targeted viruses harder to use without detection);

* don't make automatic responses to changing conditions needlessly quick; encourage human monitoring.

The OPSN architecture,
in which each network node's storage
consists mostly of a small set of "grow-only databases" ("pools of immutable items"),
lends itself to conservative modes of operation
(which any network node can choose and customize independently),
which can limit problems even when a cascading failure or network disconnection does occur:

* keep the most important code and data in its own pool, with conservative acceptance of changes;

* periodically make sure you can still run your important code (using the local database)
after freezing database changes and/or cutting your network connection;

* keep everything important cached locally (for example,
all code you care about running reliably,
all info you personally wrote or read,
and all info in your "personal wikipedia" with a high rating of likely interest to you
(under criteria you choose for this purpose));

* use a modular design which permits rolling back any deviation from the "grow-only" model
(like data removal, or changes to which code versions you use);

* keep true offline backups (but periodically test their integrity).

