---
layout: post
title:  'comparison to "Semantic Web"'
listed: false
date:   2015-11-10 17:32:33
author: "Bruce Smith"
part_of_EMSC: true
---


The goal of defining data formats with extensible and general meaning is very well known;
many systems have been proposed and some are coming into wide use,
especially some parts of the ["Semantic Web"](https://en.wikipedia.org/wiki/Semantic_Web).

In the short run,
it would probably be possible to use existing Semantic Web features
to define symbols suitable for initial experiments with
Overlaid Personal Semantic Networks (OPSN),
covering a few basic kinds of item data and relations,
rather than relying on the kind of universal symbol references described in this document.
(This would not avoid the need to create reliable references for rehostable *items*,
so it's not clear using the Semantic Web this way
would save much work in practice --
especially since an early OPSN prototype
might get by with very basic semantics (defined by a handful of symbols),
but need much innovation of other kinds.)

But for the long-term goals outlined in this document and in the
original overview of OPSN,
["a Social Network for Ideas"]({% post_url 2015-09-21-social-net-for-ideas %}),
in which OPSN could be extended
(reliably but in a fully decentralized way)
to handle general kinds of data and relations,
the Semantic Web as currently practiced has several problems,
at least one of which is serious and fundamental.
(To my knowledge,
no current system or proposal other than the present one,
whether part of the Semantic Web or not,
tries to address all of them.)

I list those problems here, and elaborate on some of them in following sections.
Nonetheless, there are useful strategies for coexistence between OPSN and current Semantic Web features,
which ought to permit some interoperation, and reuse of ontology-related work, tools, and design skills;
it's also possible the Semantic Web could evolve to solve these problems.
(For some of them, it's possible solutions already exist which I'm not aware of.)
In the last section I discuss some of these possibilities.

The problems (with using the Semantic Web to define OPSN symbols for data formats and relations):

* **unreliable definition references** -- at least in usual practice, references to 
"ontologies" (libraries of symbol definitions and related properties)
are location-based object references (URLs), not reliable immutable data references.
This has several severe effects on their reliability, as elaborated below --
in the long run it's the most serious problem
(even though for initial experiments it could be ignored).
Fixing this would be "politically difficult" --
not technically difficult, but requiring action from people with no present incentive to do it, like browser developers.
It would require implementing new kinds of URIs, encouraging their use,
and significantly revising existing development tools and practices.

* **other symbols** -- there is no provision (to my knowledge) for interoperating with symbols
defined in fundamentally different ways (to the extent that's possible) -- in other words,
the symbol-definition system is not "universal" (in the sense defined in the main post).
(Note that this is a different issue than the prior one,
which is about symbol and ontology *references* not being universal,
even when they refer to targets defined using the Semantic Web.
This might be addressable by adopting
conventions for defining symbols meant as proxies for symbols defined using other systems,
and some way of declaring certain symbols equivalent in practice,
which would affect automated uses of those symbols.)

* **forking** -- there is no accepted community practice (to my knowledge) for routine "forking" of definitions
(whether in response to errors, ambiguities, or incompatible desires for change).
(This is not a *fundamental* issue, since such a practice could presumably be described and adopted,
or (at small scales) could occur informally.)

* **no help with exprs** -- although expressions are the best and most common way to represent general meaning,
the Semantic Web has no specific provisions to help define them -- it's oriented instead around simple relations
for which automated reasoning can be straightforward. Since its symbols could be documented to mean anything,
they could in principle mean meaning-functions useful for forming expressions,
but (to my knowledge) the Semantic Web has no specific features or standard ontologies meant to help with that.
(This is only a short-term practical problem, since related tools and ontologies could presumably be added.)

* **not yet common practice** -- creating and using application-specific symbol definitions
(for example, to "formalize" formats for new kinds of data files)
has not yet become a common practice
relative to how many potential applications it has (every custom data format).
(Of course this is even more true for the present proposal, which is not even fully defined, let alone implemented.
If the reason for it is just
"not enough time having passed
for the benefits to become obvious and the needed work to be done",
then this might not reflect any intrinsic difference between the systems,
but the Semantic Web has a head start.
But to my "outside programmer's view",
this slow uptake seems more likely to be due
to limitations in existing tools or their required formats,
or to knowledge of the above problems with the Semantic Web
leading to lack of enthusiasm for using it in this way.)


## problems with location-based definition references

Location-based references have multiple problems in general,
described at length in
["comparison to location-based object identity"]({% post_url 2015-11-10-OPSN-vs-location-identity %}).
(That post also discusses why we call URLs "location based",
even though they're not tied to *physical* locations.)

All of those problems also apply to this use of them, for references to ontologies
(and more generally, to anything analogous to an "import statement" in software,
whenever it can reference a remote resource):

* **broken links** -- authors of ontologies might be more aware than the average webpage author
of the potential for future broken links to what they're writing,
and more motivated to avoid them,
but many of the reasons for them are beyond their control
(especially if application-specific ontologies become widespread),
such as management incentives or failure of organizations.
So at best this will make their URL lifetimes somewhat longer on average than the "few years" typical of other URLs --
but not "essentially forever, as long as there is interest", as we need them to be.

* **DNS** -- the potential for someone to fork DNS for political or criminal reasons
(as a whole, or for specific web resources, perhaps at the level of individual ISPs)
is just as present for ontologies or data (once they become important) as for ordinary webpages and websites.
While no protocol can prevent this or prevent laws that require it,
using reliable data references at least means that end-users could be aware of it when it happens,
unless their personal software is modified without their knowledge,
since the hashes of replaced references would only verify
if the replacement was done in the OPSN client,
or in reference-containing items it can also verify.

* **unverified data** -- the reader of a document containing a symbol
can't tell whether they're retrieving the same definition for that symbol
that the document's author (or the symbol author) assumed they would.
(This is true even if they use HTTPS,
both because that's not completely secure (as discussed elsewhere),
and because it's only verifying the connection to the server,
not whatever data the server provides.)
But definition changes will be not only possible but common,
since ontologies can be *intentionally* changed,
to fix bugs or otherwise improve them.
It's a complicated issue, since 
the typical user often *prefers* getting an updated version rather than the one that existed
when the reference was made. We discuss various aspects of this below,
and conclude that always having to reference specific versions
(of imported definitions, rules, and code -- not necessarily of everything)
will be better overall -- especially once the graph of references reaches a large scale --
and can be adapted to (by users and their tools and practices)
so that it's not much harder to deal with than what they do now, and is in some ways easier.

In the following sections we look at various usage situations,
in each one comparing location-based and immutable-data-based references,
both for symbol definitions, and for other external resources used in programming.
We also discuss some related practical considerations.


### referencing data which will be debugged or improved

In any kind of programming -- in fact, in any creative activity
where what you directly make has complex and indirect effects
(which certainly includes developing ontologies) --
it's very rare to "get everything right the first time".

So it's inevitable that
**the typical design
will go through multiple versions**
as experience is gained using it,
which *at best* will converge towards an "ideal" or "originally intended (in hindsight)" design.
But most programmers don't want
their nice new symbol to be called X/Y_version4 or X_version4/Y when it could just
be called X/Y, so whenever they're allowed to name *all* the versions X/Y
(as location-based naming allows them to do), they usually will.
(And even if they don't, nothing *guarantees* that even a name like X_version4/Y
always resolves to a fixed version.)

The developer might describe this differently (and in a sense more correctly) -- they'll say that
the name X/Y doesn't refer to a specific version at all, but to
"the latest and best, of all available versions" at whatever time it's dereferenced.
Assuming they're right that latest means best,
this does indeed cause the dereferencer to get **the best available version**...
but that version **is probably not best for all purposes!**

As a trivial example,
each test case that demonstrates one of the bugs that led the symbol's definition to be improved
will no longer demonstrate the same failure -- so it's useful as an indicator of current quality,
but not as an archive of past behavior.

A more important example comes from **an "application"**
(a program, or any other complex thing which uses the symbol)
**written by someone else**,
which intentionally used a "feature" which the symbol developer (once they knew about it)
decided was a "bug", and "fixed" -- thereby
**breaking the application, though its code has not changed.**
(There is no real time limit about how much later this might happen,
after the symbol or other resource was created -- the more, or in more different ways, something is used,
the more likely new bugs will be found; the likelihood that its developers change their mind about what behavior
would be good perhaps even *increases* with time.
There is also little safety in the argument "it's so simple we've surely
found all the bugs by now" -- simple things will be used as parts of collections (like libraries or
ontologies), so even if this holds for most of them individually,
it's unlikely to hold for the collection as a whole.
Finally, for the kind of definition meant to approximate some natural category,
the boundary of what it should and shouldn't apply to is inherently somewhat fuzzy and subjective,
so ambiguity and controversial changes of mind are very likely.)

(It could be that today's ontology developers aren't experiencing these problems as much as
most programmers. If so, that's likely to change once ontologies are widely enough used
that it's common for independently developed ones, sometimes developed by non-specialists, to need to interact.)


### version-specific references would be better

**To address this problem,**
anyone who is serious about careful definition or testing of an application
which is meant to stay reliable for a long time,
should
**make sure that whenever it imports an externally administered resource**
(whether a symbol definition, subroutine library, or anything else),
**it imports only a *specified version* of that resource,**
not the *latest version each time it's used*.

If the application's developers *still* want it to
"**use the latest version** (of the imported resource) **whenever possible**",
but in a safe way, they can support that -- whenever a new version of the external resource becomes available
(which its developer describes as "stable"),
**the application developers can change their reference** (so it imports the new version),
test their application again,
fix any bugs in it due to this change,
and (if all is well) publish the resulting new version of their own application
(whose users can decide individually whether they want the new version or not).
(Modern good practices make it **practical to automate** most of that process.)
(If their application is actually a "library" with different users of its own,
those users might then go through a similar process,
perhaps depending on whether the application's developers decided to call *its* new version "stable".)

(What the application developers **shouldn't** do is
**make each of their users be the guinea pig** for "testing" each new version
of the external resource (via its use by their application) -- but that is what
location-based identifiers for imports naturally lead them to do.)

On the other hand, if the application developers
**just want their code to keep working like before,**
neither gaining nor potentially losing
from changes to the external resources it imports,
**all they have to do is ... nothing.**
(Whereas supporting this choice
when imports use location-based identifiers is either difficult or impossible.)

There can be situations in which "doing nothing" is not viable,
like a **change in operating environment** which there is no choice but to adapt to,
but these are the equivalent of a *forced* (perhaps implicit) "import"
of a new version of the interface to that environment --
how we handle explicit imports will make no difference in that case.
Instead, when we have a choice, we can try to develop and use
environments which change in backwards-compatible ways,
if necessary by allowing explicit "import of interface version",
or as it might be described more normally,
"declaration of required interface version"
to be noticed and adapted to by the environment.
(And when we don't have a choice,
we can do our best to write code which works with all versions of the interface it might encounter,
and also look for alternative environments whose developers show more consideration.)

A related issue is when independently developed but communicating applications
are expected to all **use the same version** of some resource or interface, **for compatibility**
(like the definition of a communications protocol,
or of the calling convention to be used by compiled object code),
but that is really a special case of their environment changing
(since the other applications are part of their environment);
the fact that in general it's neither safe nor practical
to always update everything simultaneously
argues for making sure the environment can adapt to the application's interface version when necessary
(by letting it be explicitly imported or declared, as mentioned above),
which should not be too difficult to do
when it's a supported design goal of the environment's developers.

(It's worth pointing out
that both of the just-mentioned situations
in which making version-specific imports is "unsupported for reasons beyond our control"
are unfortunately not rare in general programming, but seem likely to be less common in ontology development.)


### programmers are already using some version-specific references

Some software developers have already responded to the above problems,
by providing support for some kinds of "version-specific imports" --
for example, in dependencies between projects in package managers or version control systems.

Most programming languages have not caught up with this --
import statements not only fail to mention the desired version to import,
but are specified to import something from a relative location,
rather than an absolute location such as a URL, or a data reference.
These are for the most part not oversights,
but conscious choices about what features are most convenient for references *within* a typical project --
which until recently (on the timescale of language design)
covered the vast majority of references within typical code.
(The biggest exception is references to separately distributed libraries (to be linked to as object code),
and declarations of calling conventions they should support (which are generally not possible to make, to my knowledge);
in this case the lack of explicit references to specific versions
has caused notorious problems for a long time,
even though it also has short-term benefits of the kinds discussed above.)

What is needed in a programming language is a more flexible kind of import statement
(or similar reference, such as a subroutine call into a library),
or a few statement variants,
which can use different features for intra-project and inter-project references.

For inter-project references,
it would be useful to specify both a specific version to import,
and a policy for mostly-automatically updating the code (to change that version-requirement)
when the specified version can't be found (hopefully rare) or becomes deprecated for this use.
Of course it's also important to have some kind of reliable reference to the correct project.

(For intra-project references, it's usually best to always refer to the latest version,
but an alternative to being fully location-relative would be useful,
in which one could specify a location or other symbolic name
relative to some level of containing project,
from whole project to "subproject or module" to source file.
This is only mentioned for completeness, and is not fully developed here --
it's not directly relevant to the present proposal,
so is beyond the scope of this document.)

We can hope programming languages (or surrounding environments and development tools)
will be given features like that, but in the meantime,
we can provide them in new designs for references
in which long-term reliability is a centrally important feature --
like the symbol and data references in the present proposal.


### couldn't we just use location-based references safely?

In spite of the above problems with 
imports using location-based references,
isn't there a way we can use them safely,
at least if
resource developers, application programmers, and users
all cooperate --
for example, keep using them for imports, but also (in some extra way)
specify which version of each resource should be retrieved?

There probably is (with a fair amount of trouble)... but this is
**just a way of implementing reliable data references on top of them.**
Anything which solves the basic problem is doing this, one way or another --
so the lower down in our infrastructure we can treat reliable data references as a primitive,
the simpler, more robust, and less painful things will be.

And once we're used to them (and our code has adapted), **they won't be significantly *harder* to use** --
for example, the policy described above of
"safely upgrading references to latest stable versions whenever possible"
can be syntactically requested (on specific references or in their contexts),
and automated in conjunction with development tools,
as long as testing our application is automated (which is a good idea anyway).

(We could even define operations which used existing web resources (like HTML pages with the current style of Semantic Web markup)
in a safe way -- for example, provide them along with a list of all the resources they might try to load,
and give a reliable data reference to use for each one. This list could be created automatically as part of a transition
from old to new references, until a simpler and better translation of the original page became available.)


### when symbols need to be "forked"

The above comparison between kinds of references
was focused on the *best case*, in which a definition, though changing,
is steadily "improving" towards a single "ideal version".
(Even if it never gets there, it changes along a single path, with the changes being improvements
for most purposes.)

But when a definition is widely used,
it's also common for different users (or communities of users)
to think of it as approximating *different* ideals,
with the difference ranging from slight
(as in how borderline cases are handled)
to profound (as when one concept can be generalized in several ways).

What happens then depends on how the definition (or other imported data) is used.
If it's just documentation, it might be mostly ignored -- different people might just *use*
the same symbol differently
(causing problems when those uses interact but are not fully compatible, but perhaps going unnoticed otherwise);
or they might seek to improve it (change the definition) in incompatible ways.
If the definition has a functional effect (e.g. if it's the code for a subroutine),
then users who want it to mean different things will *need* to change it differently.

Either way, if the different reasonable views can't be reconciled,
what ought to happen is for the symbol to be "forked" -- split
into "variants" (each with a new, different definition)
which can be improved or generalized in different ways.

When symbol references are **location-based references to mutable definitions** (e.g. URLs,
or any other "nicknames in a namespace" --
a nickname counts as a "location" since it identifies one piece of changeable state,
namely the definition),
at most one of the variants can retain the symbol's original name.
This has **several problems:**

* there can be disagreement about the substantive question of which variant should retain the original name.

* the variants that don't retain the name might be viewed as somehow inferior, or more different than the one that does,
even if there is no objective reason for this.

* the variant that *does* retain the name
suffers the same problems caused by a fixed name's changing definition as described above,
except this time it's more likely (for a given user) that the changes make it worse,
since they do so for each user who 
ought to be using one of the other variants.

* when reviewing code, there is no good way to distinguish an obsolete use of the symbol
(one which ought to be changed to one of the variants)
from a use which was already reviewed, and found to be correctly using the variant that retains the same name.

  * this also means it's difficult for development systems
to alert all users of a symbol that it's been changed,
and that each "old" use needs review.

When, instead, symbol references are **reliable references to immutable definition data,**
the situation is **much improved:**

* every variant after a "fork" has equivalent status,
so there is nothing to argue about except *whether* more than one variant is needed,
which is simpler -- even if a minority unilaterally forks the symbol,
the majority is likely to want to distinguish their "correct version" by explicitly amending its definition to say
it's not the "silly version" favored by the minority,
but they can only do that
if they also fork it.
That decision has little downside for them,
since they can automate the old symbol's replacement in *their* code
with the variant they consider to be the only reasonable new choice --
in fact, they can think of this as "just another routine improvement to our symbol's standard documentation".

* no variant suffers the problems of a changing definition for a fixed name.

* it's easy to tell whether each use of the symbol has been reviewed (for which variant it should use),
since all uses of the original symbol need review,
and will be changed after review
(unless it's not yet clear which variant they should be changed to).

* having universal symbol references (especially if they are part of an OPSN discussion network)
makes it practical to mark the old symbol as deprecated
in a way which every user's software development system can notice,
giving the reason for deprecation,
and some choices of variant symbol to consider replacing it with.

### when communities diverge without realizing it

It's also possible for subcommunities which don't interact for awhile
to use the same symbol in different ways *without even noticing*
(at least when they treat its definition as documentation rather than code).

**If the symbol is just a nickname** or URL, additional documentation can be supplied separately,
so **there's no strong incentive to change the "official definition"** (found at its URL) to keep it up to date
with what they're "using it to mean", let alone to replace the symbol itself (i.e. to change the URL).

Perhaps many years later, some interoperation between these communities will be attempted;
it will run into problems, but by then **the incompatible uses of the same symbol
might be entrenched** in each community and difficult to change.

In contrast, **if the symbol is a reference to its immutable official definition,**
and if updating that definition to clarify or amend it
(and replacing all uses of the symbol to point to the new definition)
is a routine and mostly automatic practice within each community,
**they will effectively fork the symbol (by each updating the definition in their own way)**
without even needing to explicitly detect the conflict.
(Each community just thinks they're improving the symbol's documentation,
because they understand that documentation isn't "definitive" unless it's part of the official definition.)

This time, **when they try to interoperate, they'll already be using different symbols,**
so there will be no interference.
(At worst, they'll need to create translators, but this need will be easily detected,
since one community's expr parsing rules won't recognize the other one's symbol.)


### ways to make immutable-definition references convenient

Of course it's more convenient to write programs using symbolic names as references
(to symbols or other external resources);
we also typically want to automatically update most instances of each reference
to follow whatever updates to the symbol we approve of, in a more central way than per-reference-instance --
maybe per-user, or maybe per-project (for the project which contains the reference).

Using name-based references (which are a special case of location-based references, as explained above)
makes this updating happen implicitly,
but has the other problems described above.
Fortunately we can create development tools and practices
so that immutable-definition-based references, which solve those problems,
can also have the same benefits as name-based references.

The basic idea is simple:

* Use name-based references (whether URLs or nicknames/identifiers) locally,
within lexical contexts (of size ranging from parts of files to entire projects)
which define those names as equivalent to immutable-definition-based references.

* Use intra-project imports, or other lexical-context-specifying methods, as needed,
to link name-definitions to name-uses within one project.
(This can be done in exactly the same way as in existing languages, if desired.
It may be useful to follow "design guidelines" to keep things organized,
but that's also desirable for other reasons.)

* Keep those context-definitions in a standardized form, also marked with an update policy,
so either manual practices or an automated tool
can review and update them when desired.

This could end up being *more* convenient than current practice,
especially if it could work at multiple (nested) lexical levels,
due to the extra flexibility and organization it would provide.

Or it could be done only at the whole-project level,
making development work within the project identical to how it is now,
but insulating the project from unwanted or untested external changes
until/unless its single list of name-to-definition bindings was updated.

Either way, automated tools could convert between local names and the equivalent definition-references.
For example, an IDE seeing a local reference of either kind could show you useful properties of the definition,
like whether it had been deprecated, and project-wide settings related to it.
Or, when pasting textual code
in a format which includes enough context to make its own name references clear
(like those described under
["universal document format"]({% post_url 2015-11-31-OPSN-universal-document %})
[UNFINISHED]),
the IDE could offer to translate those references into your own project's nicknames for them,
and to add new nickname definitions to your project as needed for that.


### coexistence and interoperation

It would be possible in theory for the formats and conventions which define the "Semantic Web"
to be modified to address the problems described above,
for example by extending its URIs to support reliable data references,
and gradually changing over to use those exclusively.

It's hard to predict whether that will ever happen.
It clearly won't happen in the short run,
so people wanting to implement OPSN should largely view the Semantic Web
as an independently developing resource
which they can use to the extent it's useful,
but should not hold out serious hope of modifying.

OPSN will certainly need hash-based references for *items*, as mentioned above,
as well as for any symbols it wants to let users extensively customize or disambiguate.
So it seems simplest to use them for all references, as envisioned in the main post.
But if there are existing data formats for things analogous to some kinds of "OPSN items"
which use symbols defined by the Semantic Web,
there is nothing wrong with viewing those as a specialized expr format (using means outlined elsewhere)
so they can be imported or exported.

Likewise, if the work already put into developing ontologies
(for the Semantic Web or any other existing system) can be reused,
there is nothing wrong with doing so.
One way is to define a mapping between some OPSN symbols
and symbols defined in other ways --
as pointed out before, we can only call OPSN symbols "universal" if this can be effectively done.
So symbols defined within existing formal vocabularies --
like schema.org or OpenGraph or Cyc or the Wolfram Language --
could in principle be used within OPSN,
essentially as if they were native symbols,
either as small expressions
whose constructing symbols define the mapping,
or as native symbols formally defined as aliasing the original symbols.
(The same thing can be done with symbols implicitly defined
by existing standard data formats, APIs, etc.)

(Looser kinds of coexistence are also possible,
such as the one mentioned above of
automatically wrapping HTML containing Semantic Web URLs
with a mapping from those URLs to equivalent OPSN data references.)

More generally, the lessons learned developing ontologies for other systems
will sometimes carry over to developing sets of related OPSN symbols;
related ideas, tools and skills could be applied to both.

