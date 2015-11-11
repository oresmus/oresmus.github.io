---
layout: post
title:  "reference to external data"
listed: false
date:   2015-11-10 17:32:48
author: "Bruce Smith"
part_of_EMSC: true
---


Often it will be useful from within OPSN to refer to immutable data hosted outside it,
like the contents of an external binary, text, or image file.

This is *almost* like referring to an immutable item already in OPSN,
since an item in OPSN can be *stored* anywhere that the
OPSN client knows to look for it (given the reference, including its location hints,
which if necessary can specify (safe) computation needed to find or extract the data) --
it need not be in a location "designated for OPSN" (or even one that's ever heard of OPSN).
For example, OPSN item data might be all or an extractable part of a webpage,
or retrieved from a conventional database or filesystem --
the hash verification ensures that either the correct data will be found
or the lookup will fail.

But it's *not quite* the same, because the found data should *be* the value referred to,
rather than *be interpreted as meaning* the value.
(The found data is not an item at all -- no item is involved.)

That is, the value (and meaning) of an external data reference will just be an array of bytes,
whose hash matches the one in the reference -- the same array which an ordinary reference would have gone on to *interpret*
as meaning something else, according to the OPSN item data format.
(That interpretation might or might not have caused any further transformation of the data --
in some cases the "something else" would be best *internally represented* using the unchanged original data --
but even if not, it would change how further use of the item would treat it.)

If that array of bytes is known to be in some other specific format, like a specific image format,
this reference can be "wrapped" (used as an expr argument)
in an expr whose combining symbol defines the transformation needed to interpret the data using that format.
In this way we can effectively refer to "external data interpreted in a certain way",
even if that way might be incompatible with the OPSN item data format
(i.e. specify a different interpretation for the same data).

As for syntax, an external data reference could either be treated as an item reference
with a special kind of in-reference metainfo (provided we remember it doesn't refer to an item),
or as a minor syntax variant of an item reference (like a symbol reference is).

