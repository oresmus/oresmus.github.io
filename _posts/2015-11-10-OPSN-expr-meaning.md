---
layout: post
title:  "expr meaning -- principles and code"
listed: false
date:   2015-11-10 17:32:42
author: "Bruce Smith"
part_of_EMSC: true
smalltitle: false
---


The purpose of this companion post
is to be more specific
about how expression meanings are derived (in principle) from symbol meanings,
when and in what sense the software should "believe" these meanings
(when they are formal statements in OPSN pools),
and how this can be matched (or fail to be)
by programs which process or make exprs
or recognize symbols.

We also
discuss various things that can go wrong, and what to do about them,
including well-known issues with formal definitions making sense,
what happens when different symbols in one expr disagree about what it means,
and more practical issues about exprs processed by code.

Finally, we give practical advice about developing expr-processing code,
and discuss "expr standards" (grammars and sets of combining symbols),
which can be used like file formats,
but developed and combined with each other in much more flexible ways.

(For more extensive practical advice and suggestions
about writing and evolving expr-related code, see a separate companion post,
["general data and code exprs"]({% post_url 2015-11-10-OPSN-data-code-exprs %}).)

First,
we'll recap some basic points from the main post, and discuss
some general points about automatic use of formal exprs.
(Most of the general points are well known;
they're important for OPSN (and thus useful to describe here), but not specific to it.)


### recap -- basics of exprs in OPSN

* OPSN uses formal expressions ("exprs") to represent the meaning of the data it stores, communicates, and manipulates
on behalf of its users -- not in the sense of the meaning of natural language text (to its human writers and readers),
but in the simpler and lower-level sense of the data items "meaning" (or representing)
text (or an image, or a computer program, or something else),
or representing the existence of certain relations between other items,
or properties of other items.

* The use of exprs is key to an OPSN system being extensible (in the kinds of data and relations it can work with)
in a decentralized way. It's extensible because any user can add new symbols and specify their "official meanings",
so people can use them to make exprs with new kinds of meanings.
The recursive structure of exprs lets them combine symbols defined by different people,
thus combining independently developed meanings into one item in a general but well-defined way.

* The meaning of an expr comes from the meaning of its symbols and "atoms" (numbers, etc),
in a recursive way based on the expr structure;
but the symbol meanings are often specified informally,
with an "official definition" consisting of natural language text (in any language) which only humans understand.
So the meaning is not "computed" -- in fact, it could not necessarily even be *represented*, except by a similar expr --
it's just an idealized concept whose purpose is to guide the writing of related code.

* A symbol (or an expr containing one)
gets its "meaning in practice" from code (used by an OPSN client, whether built into the client or not)
which treats it in certain ways,
which human programmers have tried to make correspond to the symbol's official meaning.
When they (or other users) realize they got that correspondence wrong,
they can use OPSN to discuss this, to propose improved code
(or altered definitions to use in "corrected" or disambiguated symbols),
and possibly to accept new code into their OPSN clients.


### the "use/mention" distinction, in ordinary language ...

In ordinary language there's a difference
between *mentioning* (or *describing* or *referring to*) and *making* a statement -- only in the latter case
are you claiming to believe it.

For example, if I say "he said he did it", then (by usual social conventions) I'm claiming to believe "he said he did it",
but I'm not directly claiming belief of the contained statement "he did it".

This can be thought of as a "use/mention distinction for statements"
(where "using a statement" means "making it" or "expressing belief in it"),
though it's more common to talk
about the related "use/mention distinction for symbols or words or terms",
in which "using" means "using as a reference to something, to convey meaning within an expression".
(In either case, the distinction is needed so there is some way to talk about the symbol or expr *itself*,
namely by "mentioning" it instead of "using" it.)

(To talk about "lying", we'd also distinguish "claiming belief in a statement" from "actually believing it";
for a psychologically complex being, we might further distinguish different kinds or levels of "belief".
But we won't need distinctions like that for the OPSN system being designed here.)

### ... and in formal systems

Some kind of "use/mention" distinction is also needed
in any system using *formal statements* as part of its operation, like OPSN,
or in any *theory* using them, like mathematical logic.
(Examples of meanings of formal statements in OPSN include
"person P thinks item I relates to topic T", and many others described elsewhere.)

Specifically,
any system relying on formal expressions or statements needs to make it clear which instances
(of symbols or other subexpressions)
are "used" vs "mentioned", both for proper operation of rules which help the system
respond to situations described by statements (which should only be triggered by *uses* ("beliefs") of statements),
and for correct transformation of expressions.
(For example, when replacing a symbol S1 with a disambiguated symbol S2,
to make an expression less ambiguous but not otherwise change its meaning,
only *uses* of S1 should be replaced.)

In general, a formal grammar makes this distinction by context. Within any compound expression being *used*,
some argument exprs are also being *used* while others are only being *mentioned* (sometimes we say they are "quoted"),
according to rules which depend on the combining symbol (which is itself always being *used*).
And a database of statements needs to separate
the ones only being mentioned vs
the ones actually believed (and perhaps carry further information about the level or source of belief).

### items in OPSN pools

Each primitive item in an OPSN "pool" corresponds to one or more formal statements, which are being *used* --
i.e. "believed by that pool"
(or more precisely, by some OPSN agent using that pool to store its beliefs).
As said before, this is not the same level of the concepts
"statement" or "belief" as would apply to natural language statements stored in those items (if the items happen to be text) --
instead it's about formal statements with meanings like
"this data represents text" or "the item with this data has this uid" or "this item is in this pool",
all of which could be implied by a single item (which contains text) being in that pool.

For most items, the expr serialized in their data is
(because of its combining symbol's definition) a "term", not a statement,
and their presence in a pool just means "this item's uid maps to this item, which is in this pool".
(They are presumably there because the pool's owner thinks they are interesting enough to know about,
but that implication isn't a statement which concerns us here --
if it needs to be formalized, related statements
should be expressed explicitly by other items.)

Some items' exprs *do* represent statements (due to their combining symbol),
and their presence in the pool additionally implies it believes those statements --
so they should only be placed there when that is true.
(This is mainly used for items which express opinions (on behalf of the pool's user)
about the nature or value of other items.)

Items whose presence in a pool implies a nontrivial belief
should not in general be "blindly copied unchanged" into another pool,
since in general it will have different criteria about what to believe.
Instead, if the second pool's owner (which controls all copying into that pool)
thinks an item of that kind is interesting, it can copy a modified form of that item,
which wraps the original item with a note about the pool it's copied from.
This is like person B, upon hearing person A say "X",
believing "A says X" (rather than just believing "X").
Person B might or might not then decide (according to their own reasons) to also believe "X"
(or some other variant of it, e.g. "maybe X").

(Considering items using an arbitrary expr format,
there could even be items whose meaning depends on their containing pool,
if they could mean things like "the pool this item is in has property P" --
these would have to be transformed even more carefully when copied.
This feature is not needed -- if an item needs to talk about its own pool
(or any other aspect of its context),
it can instead use an ordinary expr which explicitly contains
that pool's uid.
Also, an expr that implicitly uses its context like this
is not in a universal format (as defined in the main post),
since its meaning depends on its context.
For this reason we rule out the use of such exprs, at least for pools visible to the OPSN protocol.
(It's possible we can't *completely* rule it out -- at least informally,
we often need to refer to things like "the world"
which could in principle be considered context-dependent (since they refer to "this world", not some other one) --
so to be more precise,
we only allow exprs with context-dependent meaning
for aspects of context which we think will be constant
across all items ever occurring in one OPSN network.
If we created two *different* OPSN networks, even using identical formats
(for example, some kinds of OPSN-software test cases might create a new tiny OPSN network for each test),
we might have to consider this issue if we copied exprs between them.))

This is only an idealized description
of how an OPSN client could behave -- it's just meant to illustrate
how it could correctly copy and transform exprs
based on their meaning. In reality each client is likely to maintain several pools for different purposes,
some visible to subscribers and some private -- for example, it might have one for the latest incoming exprs,
one for exprs it's decided to keep, a subset of items worth republishing to subscribers,
and pools containing various subsets of other pools (as an optimization, for itself or for its subscribers).
It might also have private pools for the sole use of its user,
or even "internal pools" (as an implementation aid, not related to its use of the OPSN protocol)
for using exprs to keep track of "beliefs" and other data useful in its operation.

(It's also worth pointing out that a prototype OPSN client
could be very simple in the kinds of exprs it needed to understand --
it could have one class (in the sense of object-oriented programming, as used in its implementing code)
per supported combining symbol, and should need no more classes than other typical complex software.
Much of the material in this companion post
is only relevant for people who are designing new combining symbols,
as they generalize or extend an OPSN client, or a library used for adding OPSN features to existing code.)


### the recursive definition of meaning, and the need for internal context

In general, given an expr made from a combining symbol and some argument exprs,
the combining symbol's meaning
should be a function from the "tuple" (ordered sequence) of argument meanings to the expr's intended meaning.
(And if the symbol's meaning is *not* a function, or is one which has no well-defined value when used this way,
then the expr is nonsense. And an expr which *is* a symbol has the same meaning as that symbol;
the meanings of other atoms are defined by the expr format and can't be context-dependent.)

That simple model can sometimes be taken as fully correct, and more often as a good approximation.
But there are important caveats:

* as mentioned above, not all subexprs in an expr are *used* (as carrying their meanings). If they are only *mentioned*
(i.e. if they are "quoted"),
then the meaning of the containing expr is not a function of their meaning, but of their complete structure.

* meaning of an expr is always a function of its "context", even though OPSN's universal item format tries to minimize this
(by insisting the context be as simple as possible for the whole exprs (as opposed to their subexprs) of primitive items).
But there are a few ways a dependence on context is unavoidable even for whole-item exprs,
and more ways it can be needed for subexprs. These include:

  * an expr's context implicitly contains a map from symbols to their meanings,
  and from item uids to item data and meanings.
  (Like meaning itself, this map only exists in principle, as an idealized concept. In practice it will be represented
  in software by a map from uids to internal data which somehow represents some aspects
  of those data and meanings.)

    * in general this map is not used circularly (instead, it's used in a recursion from exprs to their (smaller) subexprs).
    But it's possible to try to use it circularly,
    in well-known ways (e.g. "the smallest positive integer not definable in nine words"),
    resulting in nonsense exprs
    (which is unfortunately not always obvious);
    trying to rule out such problems without excessively reducing expressive power is famously nontrivial in general,
    though it's generally straightforward
    in exprs intended only to replace conventional data structures and file formats.

  * more subtly, an expr context can contain *hints for finding* symbol meanings or item data (like the "location hints"
  which can be given in "in-reference metainfo", or like the cache of defining data needed by any OPSN-using code).
  These can't *change* the meaning (unless there's a hash collision in a hash trusted as cryptographic),
  but they can affect whether the expr is usable in practice (by affecting whether the defining data can be found).

  * for kinds of symbols which are not universal, like local variable identifiers in programming languages,
  there must be some subexprs which have a local context different than that of their containing expr.
  An OPSN expr representing code needn't use *OPSN symbols* to represent local variable identifiers --
  it might not even use exprs which matched the (often implicit) expr structure of the code
  (for example, it might just use a string literal of ordinary source code,
  in an expr whose combining symbol says to interpret that string in a specific way as code in a specific language) --
  but it will be much more useful when it *can* use exprs which match the code structure,
  which means it will need *some* subexpr for a local variable identifier,
  which means it can't fully avoid letting a subexpr context differ from its containing-expr context.

  * when code is executed, the result often depends on its environment (sometimes in unwanted ways).
  Usually this is best modelled as what it is -- the code's result is a function not only of its input
  but of certain aspects of its runtime environment,
  so if we want a (mathematical) function which represents the code's behavior,
  that function needs a parameter for the environment's uid, state, or partial state,
  as well as for the input to the running code.
  But sometimes it might be useful to think of the code expr's
  *meaning* as a function only of its runtime input,
  with *which* function depending on an "intended kind of runtime environment".


### the convention must be extensible, but clear about disagreements between symbols in one expr

So how should a system for representing general meanings, in an extensible way, handle issues like those,
noting that it would be naive to consider any list of possible
such issues, or of strategies for handling them,
to be complete?

The only practical way seems to be to let this too be extensible, in this sense:

* **it's up to the official definition of a symbol
to explain how to treat all necessary issues
in interpreting compound exprs
whose combining symbol is that symbol.**

(This means that the symbol's full meaning is not *just* a function from a tuple of argument meanings,
but an "object" which contains both that function and the answers to those issues, or statements addressing them.
In practice, to make life simpler for definers and users of symbols,
most symbols' official definitions will specify some kind of "abstract class",
which addresses most of these issues in one of a few standard ways,
leaving only one or a few things needing to be spelled out directly in that definition.)

Those issues (which any combining symbol's definition must somehow address) include:

* what is expected of a "local expr context", if this expr is to make sense in that context --
for example, it might need
certain named aspects to be defined and to have values of certain types;

* which arguments of this expr are "quoted", or should be interpreted in (specified) modified contexts --
and (if this can depend
on the meaning of other arguments -- which should be as rare and limited a condition as possible),
in what unambiguous order can argument meanings be interpreted (which must happen after their contexts are fully specified)
(as far as I know the answer can always be "interpret each argument fully before the next one, from left to right", and should be --
note that unlike when executing code, the issue here is not "side effects"
(which can't exist) but "one argument's context depending on another argument's meaning");

* and of course, it still needs to specify a function of its argument meanings, or more precisely,
a function of both the aspects of its own context that it's said it can depend on the values (and require the existence) of,
and of the argument-expr meanings as interpreted
using the contexts it specifies for them (where "quoting" can be treated as a special context
whose meaning function is the identity function on exprs).

(If there's anything ambiguous or ill-defined about *this documentation* of that convention for defining expr meaning,
then it's up to the symbol's official definition to address that too!)

It's very important that **this convention does *not* give complete freedom to symbols
to "say whatever they want"** (and be listened to)
about the meaning of exprs they're used in.
For example, if S is the combining symbol of E, which is a subexpr of E2,
S can't say that E's meaning depends directly on anything about E2 which is outside of E --
the only way E's meaning can depend on that
is via its dependence on whatever context E2 specifies for its interpretation of E.
Also, S can't "do anything" to affect the ultimate interpretation of E2
except to say what E means (which other parts of E2 will then use in their own way),
or to say "E is nonsense".
(It might be useful in practice for S to be able to say "E is nonsense for such and such reason",
i.e. to define a descriptive error message (which any code which detects this error can produce),
but that would be done via metainfo in S which doesn't affect the "official meaning" of E.)

(If some symbol S *tries* to do one of those disallowed things, by its official definition saying or implying it's doing that,
then as another part of this convention,
we say that S thereby causes the expr it's constructing (the one whose combining symbol it is) to be nonsense --
unless, of course, some combining symbol outside that expr has overridden this,
thus effectively specifying a different context for that expr -- a context which overrides the standard way of interpreting
a symbol's official definition.)

To summarize the above convention:

* **the combining symbol always "rules"** (over anything in the argument exprs it combines, about any issue).

Of course this is only a convention. It can only be enforced by human readers of the official definitions of symbols,
or by code doing things with expressions. Nothing can prevent someone from defining a symbol whose official definition
contradicts this convention, by accident or on purpose. The convention says that any expr using that symbol
as combining symbol is nonsense
(though it does *not* say the symbol is not a real symbol);
but again, someone might write code which treats such exprs as meaningful,
and even that might be on purpose,
if they disagree that this convention is a good idea.

So like everything else in OPSN, in the end it has to be enforced (or rejected or modified) by the community of users
(and in principle it could be treated differently in different subcommunities).


### implications for expr-handling code

So assuming the above convention is accepted, how does it relate to code which makes or processes exprs?

Basically, it just says that code processing or producing an expr
ought to work recursively in the obvious way which matches the recursive structure of that convention --
use contexts as two-way mappings between exprs and meanings
(with meanings represented in whatever internal way makes sense, perhaps just by other exprs in other contexts),
and give them methods to implement those mappings
by recursively operating on subexprs or internal meaning representations,
using themselves or suitable "modified contexts for subexprs" as the objects running the recursive methods.

Of course we don't *require* code to work this way --
it's just a suggested code structure which makes the code most likely
to implement meanings which match the officially defined ones
(and when they don't, which makes that relatively easy to fix),
in both correctness and "generality"
(i.e. not rejecting correct exprs for being "too complex" or "not using the structure I expected"),
and to honor the above convention for how it's ok to define expr meanings.

This code structure is not too different from common practice,
even in systems which are mostly object-oriented rather than focusing on exprs.
For example, it's mostly compatible with recognizing an expr's combining symbol and calling an associated constructor
(for a class associated with that symbol), or perhaps just a value-producing function,
which interprets the argument exprs or their processed forms.
(The most likely way it's incompatible is in generality, in the sense that such code
might accept a needlessly limited class of exprs,
but that is much better than accepting an expr in an incorrect way.)

In practice, generality bugs in expr processing will be at least as common as correctness bugs,
but of course there would always be bugs even if they weren't in expr processing at all.
OPSN will help people discuss all kinds of bugs and implement fixes,
and disambiguate the symbols and exprs when that was part of the problem.

There is also a fuzzy line between generality bugs and "limitations" which are not bugs,
and which might even be intentional.
After all, it's in principle up to each user
(though in practice, often up to the developer or maintainer of their OPSN client code)
whether to consider each symbol as meaningful, and if so in exactly what way (i.e. how to interpret its official definition).

### expr "standards" -- like file formats, but more flexible

In practice, there are likely to be common standards (voluntarily used, but named and formally documented)
which list acceptable sets of symbols and grammars
of exprs which can be generated (and should be acceptable)
for certain purposes.
This is analogous to the existence of named, documented "file formats" now,
but is much more flexible:

* it's completely practical for single exprs
to contain symbols or subexprs conforming to different (independently maintained) standards --
for example,
with each subexpr that expresses meaning of a certain type
conforming to one of the standards for data of that type
that the user of that OPSN client
considers acceptable (as expressed by their user preferences).

* expr meaning doesn't "depend on the standard" --
the standard just specifies a subset of possible exprs
(which conforming code promises it can properly understand);
it's not something which affects any expr's meaning.
This also means exprs following different standards (even for the same type of meaning) can be freely mixed
without ambiguity (and with no need for them to *say* what standard (if any) was used when creating them);
mixed-standard exprs can be automatically processed just by trying code which supports each standard in turn.
This can be done at the level of each individual subexpr, to permit fully general mixing of standards within any expr.

* the use of exprs of a universal format also makes it practical
to have formally defined and automatically used sets of transformation rules
which *convert* exprs from one standard to another;
by accepting automatic use of specified "conversion rulesets" as needed,
a user can cause their OPSN client to accept additional standards
beyond the ones directly understood by its expr-using code.
So for a new standard to become widely accepted,
it's enough to distribute rulesets which convert it to a commonly accepted standard.

* standards and conversion rulesets
can potentially be updated as often as individual OPSN clients could be,
to understand "corrected" or disambiguated symbols as equivalent to the originals (or vice versa, depending on
whether the expr-accepting code in the client
can be updated at the same time). Thus an OPSN client which uses a conversion ruleset
to accept a standard can in practice be insulated from most routine changes to symbols used in that standard,
and someone disambiguating a symbol (or defining a new one, and wanting to promote it)
can focus on getting it accepted into a relevant standard
rather than worrying about each user who might want to accept related data.

* in some cases new symbols can be given formal definitions
which suggest rulesets (or rules which could be added to them) that can be used to convert exprs containing them,
or which contain a rule to use that way (i.e. a "macro expansion rule" for the new symbol);
then anyone deciding to accept the new symbol already knows a way of handling it without needing new code.
In fact,
the acceptance of new symbols which come with "macro rules"
could often be automatic, since handling them using macro rules would be equivalent
to receiving the resulting "expanded" exprs directly.

* although expr conversion should in general be non-lossy, it's also possible to specify
lossy or approximate conversions between standards.
(Since these change expr meanings, they should never be accepted "by default",
nor should they be referred to as "conversions" without qualifying them as approximate or lossy.)

