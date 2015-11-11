---
layout: post
title:  'comparison to centralized definition'
listed: false
date:   2015-11-10 17:32:30
author: "Bruce Smith"
part_of_EMSC: true
---


This companion post compares the present proposal's symbol definition methods for OPSN
to the traditional kind
in which one organization (like an official developer or standards body)
controls all "official acceptance"
of either new definitions, or revisions to existing ones,
for a given set of symbol names.
(This is distinct, but closely related to, the situation in which one entity
controls all changes to specific *items*, as for websites like Wikipedia or Facebook.)

The biggest difference is this proposal's **complete decentralization**
of the process of defining new symbols
and disambiguating the meaning of existing symbols.
(It accomplishes this by relying on OPSN discussion to guide per-user acceptance of changes,
as discussed extensively elsewhere.)

(The "Semantic Web" theoretically supports decentralized definition of *new* symbols --
since anyone can define a new ontology by posting it at a "permanent URL" --
provided we ignore the issues discussed in
["comparison to Semantic Web"]({% post_url 2015-11-10-OPSN-vs-Semantic-Web %}),
e.g. that there is *no such thing* as a "permanent URL".
As argued there, ignoring those issues
amounts to taking seriously only short-term (few year) periods of development,
and has other serious problems.
Even so, if we do ignore them,
then the symbols defined by the Semantic Web
can be extended (though not necessarily *disambiguated*) in a decentralized way.
Thus this post's comparison is not mainly between OPSN vs. the Semantic Web,
but between OPSN and the more traditional "fully centralized" ways
of defining and extending formal meaning.)

Full centralization of defining meaning
comes with at least one unavoidable problem --
it often takes too long
for a small group to effectively define and share new symbols,
especially for experimental or niche uses --
in fact, when they're a minority of users and have specialized concerns,
there's no guarantee they'll ever succeed.

Another basic problem with centralized definition is the inability of a subcommunity
to unilaterally "fork" a symbol, if there is insufficient consensus that this is needed.
And even if that consensus exists,
there can be unresolvable arguments
about which variant should be considered
"the same as the original".
(These problems are also discussed extensively in
["comparison to Semantic Web"]({% post_url 2015-11-10-OPSN-vs-Semantic-Web %}).)

These problems get worse as the network grows.
They result in much meaning which *could* be formal
being exchanged in informal and perhaps inconsistent ways,
since there is no efficient-enough mechanism for replacing informal with formal expressions
as their usage becomes more common,
or for correcting issues of ambiguity in the accepted definitions of formal symbols.

To address those problems we propose the scheme described in this document,
in which anyone can define and make new symbols
(either for new purposes or to disambiguate existing symbols),
and participate in discussing the meaning of existing or new symbols,
and in which the acceptance or further improvement of symbols and their definitions
is determined by the aggregate of individual choices by other users and subcommunities,
who are encouraged, but never forced, to come to consensus on symbol meanings in practice
(i.e. on what commonly used code treats them as meaning),
and conversely are always allowed to use their own definitions
among whatever subgroup of users is willing to support them.

Of course this choice has tradeoffs, but they can be dealt with:

* Instead of lacking defined symbols to express a desired meaning,
there can be too many to easily choose from,
and there is no "official standard" (defined by the system)
about which symbol is best for what purpose; sometimes
different people will use different symbols for the same meaning
(but determining whether they can be considered interchangeable will be nontrivial).

  * but nothing prevents external bodies from making standards in how to use the new kind of symbol,
  like the "expr standards" discussed elsewhere,
  which people are free to use if they wish, and/or to require of other users who want to interact with them;
  note that adherence to a standard can be machine-readably declared
  and (to the same extent as with other systems) validated.

  * also, nothing prevents symbols defined in other ways,
  or which are part of existing formal vocabularies,
  from being used within this system (this is also discussed elsewhere).
  
Natural language and jargon (and other aspects of culture)
already permit anarchic invention of symbols (and of less formal carriers of meaning)
and resolving of definitional issues,
and that seems to basically work, and to permit evolution and specialization in the ways described here.
(The extra formalization in this system should make it work better.)
And the larger and more diverse the overall network grows,
the more necessary it seems likely to be
to permit decentralized development and decisionmaking
about formal meaning.

In the end, it seems clear that a decentralized system like this,
for defining new symbols and their meanings,
will be better than *only* having centralized systems for that purpose,
for enabling both experimentation with and gradual improvement of new symbols,
and gradual useful formalization of all kinds of exchanged data.
It's also clear that to the extent that users *want* centralization, either worldwide or within subcommunities,
this proposal doesn't prevent them from having it -- it just refrains from trying to force it on them.



