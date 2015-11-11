---
layout: post
title:  "reference to external object"
listed: false
date:   2015-11-10 17:32:45
author: "Bruce Smith"
part_of_EMSC: true
---


To refer to an external *object* (as opposed to data or an OPSN item),
we just define a symbol which *means* that object,
or create an expression which (due to the meaning of its parts,
including the combining symbol) means it.

If our goal is just to be able to ***discuss* the object**,
it's sufficient for that meaning to be defined informally
by the involved symbols, provided this definition is clear and "correct" (matches its author's intention).
If it's discovered later to be incorrect,
the new symbol or expr can be disambiguated (replaced with a better-defined one) as discussed elsewhere.

(The only difference in the situation we end up with, compared to just discussing the same object in ordinary text,
is that OPSN "knows" which object is being discussed (and in exactly what text), in a formal way. This might be useful,
but is often not necessary. It's a bit more formal than just "tagging text to say it's related to this object".)

If, instead, we want our OPSN client code to ***interact* with the object**
(for example when running a program also described within OPSN, as the meaning of some expr
whose combining symbols are designed to help express programs -- or alternatively, if we want our OPSN client to use the object
in place of some "built-in" object
it commonly interacts with, like a local database for caching item data), it's more complicated:

* either someone has to
write code to interact with the object
(guided by the informally defined meaning of the involved symbols),
and then
either build that code into new client code we can use,
or express it in some form the client can already understand;

* or
the symbol that means the object needs to be given a *formal* definition (or have formal metainfo attached)
which can be interpreted as a program (for interacting with the object)
already understood by the OPSN client code.

Nonetheless, this will often be worth the trouble, since it will be
a useful way to extend both OPSN client code, and the utility of programs described as OPSN data.

