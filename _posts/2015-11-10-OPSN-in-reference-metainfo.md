---
layout: post
title:  "metainfo in a reference"
listed: false
date:   2015-11-10 17:32:51
author: "Bruce Smith"
part_of_EMSC: true
---


Sometimes a hash-based reference should come with **"in-reference metainfo"** -- extra info about the data it refers to,
which is stored in the reference itself (rather than in its target)
because it's useful before the reference is "followed" (i.e. before the data is retrieved).

Most references don't need that kind of info,
and when needed it could in principle be supplied separately (or the same effect could be achieved in a higher-level way).
Even so, we'd like the option of including it in the reference itself,
both for simplicity and so the info doesn't "get lost" when the reference is copied.

(Including such info in a reference makes the reference different,
but doesn't make the target item or its uid different.
Thus it can be added or removed,
when copying a reference into a context where its new holder
might need more or less such info,
without changing what the reference refers to.
This will typically be done automatically when copying references into different contexts,
so (as an internal optimization)
they can be stored locally
as an abbreviation depending only on the uid,
with all (equivalently-trusted) metainfo about a given item
stored in one place,
regardless of which original reference to that item it was found in (or of some other way it was learned).)

(As for syntax, nothing is wrong with representing a reference with metainfo
as a compound expr which combines a "plain reference"
with the metainfo, as long as we usually consider the entire expr a "primitive reference"
for most purposes.
Alternatively, especially for the most important kinds of in-reference metainfo,
we might provide a special syntax for specifying them.
Both systems could coexist on a single reference.
Which one is used should be considered an issue of a specific syntax rather than fundamental --
that is, a code library (for use in an OPSN implementation)
for finding a reference and working with its in-reference metainfo should treat both kinds uniformly.)

There are a few basic uses
a reference-holder might have for in-reference metainfo;
here are some examples of each
(but clearly the kinds of this info should be an extensible set,
which means each kind should be specified with its own symbol,
perhaps used as an expr constructor for a "piece of metainfo"):

* help **decide whether to retrieve** the referenced data:
  * warn if the data is unusually large or costly, and/or useless in certain common environments
  * warn if the data is unsafe to handle in some environments
    (e.g. a file containing a virus which exploits bugs in common decoders for the file's format,
     or data demonstrating a hash collision for a hash function commonly treated as cryptographic)
  * warn if the data has special licensing requirements, or might be illegal in some jurisdictions

* help **prepare for having** the referenced data:
  * warn if the data is private, and thus comes with special responsibilities for handling it or for how it can be retrieved
  
* help **find** the referenced data:
  * provide a location hint, like a URL,
    or a reference to a better-known object which could help locate the data (like a project it's part of)
  * provide an additional verifier that the correct data was found, such as a different hash
    (with variant algorithm parameters, made anew for each newly shared reference)
    to help a community detect hash collisions on critical shared data.

(Note that in-reference metainfo is related to, but different from,
ordinary metainfo about an item (typically supplied separately from the item)
which relates to which users
are interested in that item or might want to avoid seeing it. In some cases such info might be reflected
in the in-reference metainfo for warning about often-useless large items,
but in general, making use of such info is better done at a higher level.)

If the metainfo in a reference
fails to include info that an OPSN subcommunity thinks it ought to,
they can deprecate the reference (not the item it refers to)
and recommend replacing it with a more informative reference to the same item.
