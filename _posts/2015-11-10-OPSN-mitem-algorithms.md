---
layout: post
title:  "sketch of mitem algorithms"
listed: false
date:   2015-11-10 17:32:27
author: "Bruce Smith"
part_of_EMSC: true
---


### single-author mitem

A "single-author mitem" is one which only one person (on one computer) can change
(which makes it simple to implement),
but anyone interested can subscribe to (or receive via other people) and read.
(Anyone can also make a universal reference to the mitem.)

The most fundamental kind of single-author mitem is a "growing file"
(or almost equivalently, a "growing sequence of chunks of immutable data"):

* passing on an incremental change just means sending more chunks
(plus an optional hash or signature of the whole file so far).
The primitive items which represent the mitem state
can be formal statements about its chunk data,
but they needn't be stored explicitly, since they can be reconstructed from any equivalent form of the data.
In fact, at the implementation level a "growing file" might be a *more* primitive construct than "primitive items",
used directly to represent part of a stream of updates from one user to another.

* merging partial information from multiple sources just means picking the longest partial file
(or combining chunks, if some sources could have sent recent parts but not older parts)
(and optionally verifying the common parts agree).

* the only important metainfo (which is special to this type of mitem)
would be an estimate of
author intentions about statistics of future extensions (how soon, how often, how much at once, likely final length).
It shares with all types of single-author mitem a binary status of "finality"
(whether the author has irrevocably renounced the ability to further change it -- in this case that means to extend it),
and (when available) an estimate of update propagation delay along various paths;
and with all items, it shares potentially arbitrary metainfo about its properties or purposes.

(The metainfo would not normally be part of the mitem data, though sometimes it could be --
I am just listing "the things someone might want to say about this kind of mitem"
(whether or not they were the mitem author),
which they would normally transmit outside it.)

From a growing file, it's easy to derive any other type of single-author mitem.
For example, if the file content is divided into separate "records",
each one can be a new variable value;
or for more complex types of variable data,
each record can be a "value-modification command" (applied to the result of all prior commands).
In this way an arbitrarily complex changing value
(like a source-code repository, or a set of immutable items with adds and removes)
can be "broadcast" from one author to all interested users as a sequence of incremental changes,
with each extension of the growing file mitem consisting of a convenient batch of more changes.

Fundamentally, that kind of mitem just represents an ordered sequence of values,
with no notion of "time" except for the order itself
(which is only meaningful within one mitem -- there is no ordering between values of different mitems).
If something like a "value creation time" is needed, it could be an explicit part of a fancier value.

More generally, any transformation of mitem values (extension, filtering, condensing, combining) might be used
to define one single-author mitem as a function of one or more other mitems (by the same or different authors).
Note that this is a higher-level concept of "definition" than the one discussed earlier for setting up an mitem and its muid;
it includes both making the mitem and setting up some process (run by the mitem author)
for adding values to it, and it can be done "circularly" so that several mitems (considered as growing value sequences)
can be mutually dependent. This has several uses, including:

* to implement something like "functional reactive programming"
(though when it's distributed, the delay around any loop of dependent mitems can't always be limited);

* as a special case of that,
to implement the "combining of weights" at the heart of the OPSN discussion system,
in which each person publishes one mitem describing their current opinion about some set of weights,
which they compute from similar mitems published by their desired influencers (combined with a few choices of their own);

* for each of several interacting single-author mitems
to represent its author's view of one multi-author mitem they are editing together --
this can be used both for realtime collaborative editing
and for updates to a distributed version control repository;

* the same, but for the important special case of "multiple authors" who
are really the same person using different computers;

* to condense a "transient data" mitem into a "summary" or "highlights" mitem --
that is, given an mitem containing a stream of high-volume data whose useful lifetime is brief
(like moment-to-moment player position in a game, or complete transient state in text-editing),
to define a compacted version of it containing just the part that matters after some time has passed
(to be broadcast farther and/or kept longer).


### multi-author mitem

As a simple example of an mitem changeable by multiple users (with details only sketched here),
we'll define below 
a distributed data structure
which could be used for an mitem algorithm
handling arbitrary changes to a single value, and able to represent
"branching", "forking", and "merging" (by one or many users acting as "authors").
Since a single value can name a version of a complex value (like a set or graph of objects),
an algorithm like this could be sufficient for many purposes.

This sketch has obvious similarities
to well-known algorithms for distributed version control and realtime collaborative editing.
(In fact, I wouldn't be surprised if it's equivalent to some published algorithm,
since it's inspired by several of them, but I haven't followed recent related work closely.)
A real implementation could benefit from (and be further improved by)
the large amount of existing work
on such systems
(and with luck, could also benefit from their existing code).

First some terminology:

* value id -- a reference to a single consistent state (plus associated metainfo specific to the mitem)
of the variable an mitem represents the history of
(recall that the history, a distributed data structure, can contain per-person variation as well as time variation);
the consistent state itself can have a complex type (like a set or graph), but a value id can just *name* that state
and thus be treated as simple;

* transition -- an arrow or edge from one value id to a possible next value id
(according to at least one potential author of the mitem) --
the transitions must form a directed acyclic graph ([DAG](https://en.wikipedia.org/wiki/Directed_acyclic_graph)), which means
they can't "go back to the same value id as before",
though the difference is allowed to be only in the metainfo, not in the variable value itself
(and also, this rule doesn't prevent some *user's opinion* of the current value id
from changing back to a prior value);

* divergence -- when there is more than one transition from a value id
(even locally or transiently -- thus divergence is unavoidable in general with any form of distributed editing);

* merge -- when transitions go from different initial value ids to the same next value id;

* fork -- when a divergence is resolved by becoming a recognized permanent split into a new mitem
(but sometimes only some users recognize the fork, with others considering the same divergence to be a branch,
and users can change their mind about this);

* branch -- when a divergence is temporarily edited as if it was a separate mitem,
but is intended to eventually merge back into the original one (and is always stored internally as part of the same one;
this is the default interpretation of a divergence that's not immediately resolved, but any user can reinterpret it as a fork).
In some systems of software development, it's common to make a branch for work related to one feature,
to be merged with the "main branch" when that work seems complete.

In the distributed data structure for this example mitem algorithm,
the (immutable) primitive items (a growing distributed set of which represents the mitem's complete history) include:

  * *value id symbols* --
symbols specific to this mitem, with a formal definition expr including an mitem-variable-value and some *feature exprs*;
a value id symbol (often called just a *value id* below) represents the state in which
"this mitem's variable has this value, which is known/considered to have these features";

  * *feature exprs* -- symbols or exprs included in value id definitions to indicate features or status of their values
  (but allowed to be shared between mitems);
  used to indicate
  "having issue X fixed or implemented", "in branch B", or "intended to become part of fork F";

  * *transitions* -- arrows between value ids, possibly with metainfo useful to the algorithm;

  * *endorsements* -- expressions stating that user U has feeling/intention F about transition T
  (optionally replacing prior endorsement E by the same user),
  used to let users know each other's plans regarding accepting transitions
  as part of their opinion about the mitem variable's "current value".

The algorithm is, roughly, for each user (or more precisely, their software system, since this should be mostly automated)
to see how other users are navigating or intending to navigate
the DAG
of transitions, and decide what to say about that themselves.
Methods similar to "blockchain algorithms", or adapted from implementations of realtime collaborative editing,
could be used to keep many users on "roughly the same page";
when that was not working, it might be an indication that some users ought to branch or fork the mitem.
To do that,
they (or other users, then or later)
can define new value ids containing appropriate *feature exprs* indicating that branch or fork,
making it easier to resolve outstanding conflicts (since a branch or fork is one way to resolve them).

(Once a fork's existence becomes stable,
a new mitem can be made which tracks it, which can gradually take over from the parent mitem;
alternatively a new mitem could be defined in such a way that primitive items used to represent it
could be viewed as completely equivalent to primitives in the representation of the parent mitem.)

There can be per-user and per-mitem preferences about things like how much to favor merging over branching or forking,
and how automatic or manual merging should be.
These can depend on a user's relationship with other users doing editing,
on how carefully they wanted to track changes,
and of course on the type of data they were editing and its purpose,
such as realtime editing of shared data, source code version control, wikis, or games.

