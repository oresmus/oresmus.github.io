---
layout: post
title:  "general data and code exprs"
listed: false
date:   2015-11-10 17:32:18
author: "Bruce Smith"
part_of_EMSC: true
---


The system described in the main post --
an integrated combination of new data formats, network protocols, and conventions --
is designed to support "OPSN discussion",
using a graph of archived data
in a decentralized network as reliable and long-lasting as
the society using it.
It achieves this with an extensible universal data format,
and by encouraging data to be rehosted by its interested users,
so the data can outlive any hosting platform
and maintain its meaning and connections with the rest of the graph.

But those unique properties,
not yet available in any existing network or data format,
have other potential uses too --
OPSN's **universal expressions could represent many kinds of data**, including math or code,
easing automatic transforms and interoperation,
and enabling new kinds of collaborative development.
Adding universal expr support to existing open-source projects, or harvesting code from them,
could help lots of small but useful functions and "microformats" work together,
which exist now, but are scattered among many projects and can't easily be used together.
(Exprs which refer to those functions can ultimately develop into a kind of "universal portable code"
which can run in many environments, as discussed below.)

(The properties of OPSN's **universal data references are also useful for general data**;
developers dissatisfied with existing ways of letting their users store, access, and share data
might be interested in using them within any project.
This is important,
but is discussed elsewhere,
and has the same implementation for OPSN as for any other use,
so it needn't be elaborated here.)

These more general uses of OPSN expressions
might be beneficial even before OPSN discussion itself is implemented
(though using them together will greatly enhance both),
and even if, at first, only a few interested developers
are supporting them in their open-source projects.

This "advanced topic" post discusses how to develop and use the
"OPSN format, protocol, and network"
for use with **general expr-based data formats**
and **"open-source code exprs"** meant for a variety of applications
(most not directly related to OPSN),
and **how this can "get off the ground"**,
even though its main benefits are proportional to network size and diversity.
(The post on
["possible prototypes"]({% post_url 2015-11-31-OPSN-prototypes %})
[UNFINISHED, not yet posted] also relates to this.)

(If OPSN data or code exprs come into use before OPSN discussion is implemented,
they could also make that easier to develop,
both technically and by providing a ready community for early OPSN-powered discussions.)


### why choose OPSN exprs for new data formats?

What matters for this system's overall success as a general data format
(for wider use than just supporting OPSN discussion)
is whether enough people find writing code supporting this general form (of extensible universally-meaningful exprs)
to be worth the trouble, compared to supporting other kinds of formats for the same data.
(But even a few people taking advantage of it,
for example when adding scripting or plugins to their favorite open source project in ways described below,
or even just data import or export features,
should find that they can **make their separate work compatible more easily**
than otherwise.)

Compactness of the data shouldn't be a big issue in the long run,
since most new or existing formats can be treated as local abbreviations for certain exprs,
and the translation can be done by automated tools,
or completely avoided (via compilers, or using operations written for the compact format).
We can also use
[references to external data]({% post_url 2015-11-10-OPSN-ref-external-data %})
wrapped in an expr which specifies it as having a specific non-OPSN format.
(In the short run, most prototype applications don't need to process enough data
to worry much about its absolute size.)

Speed of data processing is a bigger issue for some applications, but even here we can imagine compilers
which help in the long run, and in the short run
there are many applications
where speed is not a dominant factor
compared to flexibility or development time -- for example,
a useful prototype of an OPSN discussion tool,
or many kinds of IDEs or other design environments.
(It's also often possible to extract the speed-sensitive parts of an application
into data processing primitives which can then be controlled at a higher level
in which flexibility matters more than speed, since the extracted parts remain fast.)

Since the system of universal exprs in OPSN makes it **easier to extend datatypes and use them together** --
for example, when a single expr includes symbols developed independently of each other,
which combines them in a way not generally possible
for independently developed file formats --
**it will help smaller pieces** of separately written code **be usefully combined.**
This should **speed up the collaborative development of open-source code**,
since the simpler something is, the easier it is to understand how to use it,
and how to interface it with other things, and therefore to reuse it
(not to mention that smaller things are easier to write in the first place).

So if an open source project (or a fork of it) reexpresses some part of its functionality,
originally written using custom internal datatypes,
as separate modules which process and produce universal exprs
(perhaps based on new symbols defined to match those custom datatypes),
it makes it easier for other people to either reuse those modules for other purposes,
or to write new modules which the original project can use in their place
(like "plugins", except potentially much simpler when desired, like single functions).

### harvesting "code exprs" from existing code

In this way we can hope that
**existing open source code can serve as a rich source of useful functionality**
in the form of simple datatypes, associated expr-based formats, and simple functions between data expressed that way,
so that OPSN clients can gain the ability to run "code exprs" expressed in terms of those datatypes and functions,
by incorporating modified pieces of that open source code (or in other ways discussed below). (The simplest "code exprs"
are just formulas made of exprs
whose combining symbols give universal names for variables or functions
computable by the application supporting them.)

Anyone wanting to add simple customization features to an existing application,
like executable formulas for use as a more general "user preference" value,
can choose to do this by adding support for code exprs of that kind,
and expressing whatever functionality they were going to build in anyway in those terms.
(For a highly experimental application, like a graph viewer for OPSN data,
letting users enter formulas
for things like detailed node-filtering rules
or layout and coloring influences
can also greatly speed up the experimentation needed during development.)

Then if several applications (written in similar-enough languages and frameworks)
each define a collection of operations for this kind of use,
it's trivial to merge that code so they both support all the operations originally present in any of them.
(This merging of code can be done using OPSN or in traditional ways.)

Initially this combination of functionality, though trivial, would have to be done manually,
with each application supporting a "curated collection" of code expr implementations.
But later there are various ways of automating and extending this (some discussed below),
some of which effectively allow users as well as app developers
to extend the set of supported code exprs and their implementations
which are available for use in their apps.

The fact that
**independently defined operations will have distinct universal names**
(even if different ones might have the same "nicknames")
makes automating this possible without introducing compatibility bugs.
Of course the UI does have issues related to nickname-overlap,
since to be useable it needs to translate between nicknames and universal names;
but this can be given a general solution in which
**users specify namespaces their UI should use,
but the UI stores the formulas they enter in universal form**,
independent of those namespaces.

For some applications, it would be useful to let these 
"code expr formulas" be combinable into larger exprs,
expressing more general programs rather than just formulas,
using some specified semantics, thus
**effectively defining an extensible expr-based programming language.**
The combining symbols for that, which express "programming primitives",
could also evolve according to OPSN discussion and symbol disambiguation.

It would be desirable for the types of data and interfaces
to be given symbolic names and properties,
used when combining them to
**enforce interface-matching requirements or insert adaptors**
between different interfaces.
This too (both the interface property definitions and the combining code that honors them)
could evolve by OPSN discussion
of the meanings and implementation of the involved symbols.

### higher-level languages can coexist

The most **likely initial result** of all this,
even if it was only done by a few cooperating large open-source projects,
would effectively be a
**straightforward language implemented interpretively**,
perhaps procedural (with a functional subset),
or perhaps related to "functional reactive programming" (or both, depending on user interest).
But there would be
**good potential for efficiently compiling this language**
(or popular subsets of it),
using techniques like in the ["pypy"](http://pypy.org/) project,
since the semantics would have the same
**self-contained permanent documentation**
as all OPSN symbols and exprs do.

(Specialized sublanguages might also be compiled by combining "code snippets" written in existing languages,
already a common practice for some kinds of code within single projects,
like GLSL shaders for advanced graphics.)

The language that would gradually develop this way
would be high-level in the sense of built-in datatypes and operations
(since any existing open-source library could be used to extend those,
so they'd be numerous and sometimes involve complex datatypes or code),
but lower level in overall language structure.
It would also lack a user-friendly native syntax.
(Uneven support for its "primitives" would be a big issue at first,
but has potential solutions discussed below.)

But **higher-level languages, with arbitrary syntaxes, could be defined on top of it,**
by implementing compilers which transformed their exprs or other source code into lower-level code exprs.
These could imitate existing languages, or be entirely new.
Multiple higher-level languages could coexist indefinitely;
they would naturally interoperate whenever their interfaces were compatible
(which would be common, due to the use of OPSN discussion to organize all this
and to define named interface types and properties).


### towards truly portable code

The biggest difference between this and existing languages would be its potential "universally portable" nature
(which would apply equally to the high-level languages defined on top of it).
By this I mean both "universal" in the sense defined in the main post,
and "portable" in the usual sense of having wide compatible support --
but eventually to an extreme degree,
due to long-term incentives for popular code exprs to be widely supported,
their smallness as units,
and **community mechanisms to encourage portability and make it practical**.

Initially there would be as much lack of perfect portability as with any other open-source project.
But whenever some code expr behaves differently in different environments (in an unwanted way),
this is either an implementation bug or an "ambiguity in symbol meaning" (for the symbols constructing code exprs);
it can be resolved in the same way as for other kinds of OPSN symbols (as discussed in the main post).

The OPSN symbol-disambiguation mechanism also allows any reasonably-sized subcommunity to **prevent "bit rot"**
(gradual incompatible changes in meaning of subroutines their code depends on)
even if the larger community doesn't bother to.
In the worst case (if they were always trying to write modular, portable code),
they might have to fork their symbols
just to keep them meaning what their official definitions say they mean,
and add new adaptors due to changing aspects of commonly available runtime environments,
but they should only have to do this once per symbol or per runtime-environment change.

**For popular code exprs, there would be incentives to support them widely**
and take the trouble to resolve ambiguities;
the barriers to interested users doing this
would be smaller than for development of traditional open-source code,
due to the decentralized nature of the system
and the possible smaller units of administration of implementations (single functions rather than entire projects).
Code exprs that end up without enough direct support
could be given translation rules to reexpress them in terms of better-supported ones.

The symbols used to combine code exprs into larger ones
would feel the same design incentives as those implementing primitive types and operations --
popular ones would tend to be supported more consistently and widely --
but these would have an additional incentive to be as compatible as possible with a wide variety of other code.
This favors simple interfaces, and ways of combining code which are inherently modular,
so that separate parts don't interfere;
thus **the resulting code would ultimately be more modular** than is typical now.
(It is also likely to be more parallelizable, since uncontrolled side effects or other order-dependencies
are not only less simple as interfaces, but a main cause of various kinds of bugs and lack of modularity.)

Ultimately, what this would feel like to developers
would be the existence of **a few large "meta-projects",
each collecting a large amount of compatible functionality** from numerous smaller projects;
using a meta-project's types and functions or adding new ones
would be much like extending a large open-source code base they know well,
but the use of reliable version-specific references to interface types and code
(at least after well-designed, modular code won out over its alternatives),
together with separate administration and free choice among the component projects,
and the basic semantics allowing "load only the code you need",
would eliminate the main problems which prevent
"putting everything useful into one huge project" from working in practice.
(This is discussed further below;
the advantages of version-specific references are also discussed more fully in the post about
["comparison to Semantic Web"]({% post_url 2015-11-10-OPSN-vs-Semantic-Web %}).)


### security and reliability for those who want it

Those developers and users who understand the basic idea of
["capability security"](https://en.wikipedia.org/wiki/Capability-based_security) --
using the same boundaries and modular structure for security-related permissions as for other functionality,
so that object references and appropriate associated permissions are bundled together --
could use OPSN to rate code-expr designs and implementations accordingly,
and try to restrict their usage to those they considered cleanly designed
and compatible with secure implementations.
(Of course, OPSN discussion could also be used to discuss and rate
the security of specific implementations of all kinds of code exprs,
as well as other measures of quality.
Many users would pay some attention to that,
and users could form subcommunities
which rated code using whatever priorities they wanted.)

None of these good properties would happen "by magic" --
they would happen because the kinds of interfaces and semantics which lead to this,
which are **already reasonably well understood** by a fair number of programmers and language designers,
would eventually mostly "win the competition" for wider use of code exprs with good properties.
This would happen more quickly than it does now, due to the smaller units of competition,
and the greater ability of both end users and intermediate "bundlers of implementations into standard packages"
to make individual choices without breaking other code.
Perhaps this would only happen within a subcommunity of users
who prioritize safety and reliability highly enough,
especially at first (before good compilers were implemented);
but the good support for specialized subcommunities in OPSN means it would still happen.


### code exprs containing larger chunks of existing source code

Whenever it's practical to implement existing programming languages in **"sandboxes"**,
OPSN clients can also safely run a bigger kind of "code expr" consisting mainly of conventional source code
marked with its language, and some details about how to interpret it and what kind of interface it needs.
(The [Caja project](https://developers.google.com/caja/)
is a serious attempt to do something like this with combinations of HTML/javascript/css
for use primarily in web applications;
it preprocesses the code-combinations (which a modern browser will then compile)
so they can safely be considered isolated from their environment except in explicitly desired ways.
I think its main complexity comes from the complexity of the external interface it supports
(which includes a web browser's
[Document Object Model](https://en.wikipedia.org/wiki/Document_Object_Model)),
so doing this kind of thing for simpler interfaces ought to be easier.)

Besides distributing code exprs containing large chunks of code directly,
it's also possible to define exprs
which refer to code already stored in **conventional repositories or "package managers"**,
as long as they provide enough info to extract a specific version of the code
and verify its hash value,
and the interface info mentioned above needed to run them properly.
The extracted code can either be run in a sandbox,
or (if trusted) used in more conventional ways when compiling a larger application.
(By making this a reliable data reference as we've defined elsewhere,
the code can also be "rehosted" separately from its original location.)

Even without use of sandboxes,
the security situation is similar to what's common now among open source projects,
in which a community evaluates their safety along with their other qualities;
but the ability to evaluate and discuss such code is improved
by the fact that the code references are inherently version-specific,
and test runs are inherently better-defined and reproducible.


### wider support

Once a lot of useful code exprs are defined and runnable in a reasonably portable way,
it will be attractive to existing platforms
(like large applications, not necessarily limited to open-source or user-run applications)
to become able to accept "plugins"
represented as the same kind of code exprs,
or defined subsets of them which are safe to support and especially portable.

And once *that* happens, it will be attractive to users of the remaining code exprs
to get them reexpressed in terms of the more commonly accepted ones,
so code that uses them can be automatically converted to work more widely.

(This would be done by
developing expr-transform rules which
effectively macro-expand or "compile" the original code exprs into the more widely supported ones.
Of course it's also possible just to manually transform individual uses of code exprs, but that's more work and more bug-prone.)

### these things are improving now, but code exprs can speed that up

The general trends described here
towards modularity and portability of common functionality are already happening,
at least in an abstract sense and in some cases,
at the level of operating environments, programming languages, libraries, file formats, and application programs,
so that more functionality gradually becomes more available in a portable way. (But because those things are all large,
they evolve more slowly, and are often difficult to use together.)

On the other hand, if you 
ignore issues of portability,
widespread availability,
or easy reusability of components,
more open-source functionality is becoming available at a *huge* rate.
Most of it is too new to have been affected directly by the mentioned trends,
which means much of it is *not* yet very portable or modular.

So, to compare those ways of defining
"the rate of increase of availability of useful open-source functionality",
there is a large and probably increasing gap between "what's easy to find and use"
and "what's merely *possible* to find and somehow use", even though both are growing.

The strategies outlined here could be used to help close this gap.
By breaking functionality into smaller pieces,
supporting a standard for data and type/interface declarations (universal exprs)
which helps those pieces interoperate,
and expressing portable code and calls to it in terms of "code exprs",
it should be possible for independently developed or maintained smaller pieces of code
to be used together more readily,
greatly **increasing their rate of evolution and improvement**,
and even more importantly, **tying them all together**
so that **independent functions or projects using conceptually compatible data
have a good chance of being interoperable.**

Even if developing this kind of highly portable and connected code
takes several times longer than developing conventional code,
the rate at which *new usefulness* (to the average potential user) is produced will be vastly higher
(once the network containing this code has a reasonable size).

### the "graph of all code"

If the development and use of code exprs becomes widespread,
it could catch on for open-source functionality
in some of the same ways OPSN discussion could catch on for discussion topics --
there would be **"one place to look" for the best code and data formats related to given topics,**
and a high motivation for new code to be inserted there (often as small pieces like single functions),
and made interoperable with whatever is already there (by using existing interfaces and datatypes,
and being written as portably as possible).

One of the ways to look at this code would be as a graph-like structure
in which nodes are datatypes and edges are code that can consume or produce that kind of data.
(The structure is really a kind of "hypergraph", because edges can have more than one input or output,
and everything in it is also an OPSN node
so it can support discussion and topic tagging and be involved in relations.)

It's also possible for "coarse edges" to represent conceptually-described transforms
(or just wishes that they exist),
and be associated with "fine edges" which represent specific pieces of code that claim to implement them.
So each piece of open-source code (whether a single function, class, module, or project)
can eventually connect both to each datatype it uses or produces (or each kind of interface it can connect to),
and to each purpose it might help serve -- and connect to all these at both specific and general levels.
This also means you can browse the graph the other way -- from general purposes or goals,
down to code (in all stages of development) which might be useful for them,
or from conceptual descriptions of datatypes to specific datatypes to code that uses them
(or projects working to make such code),
also linking to OPSN discussion about that code or those projects (once OPSN discussion is implemented).

Another way to think of this is as
the effective merging of lots of open-source code into "one big project"
made of many small interoperable parts
(datatypes and functions, some of which can be used as classes and methods).
Their administration and maintenance would be divided into many smaller projects, much like now;
but they would often work together as well or better than code written within single projects does now.
Unwanted duplication of code or of purpose would be much better addressed than now,
due to the "one place for each concept" property supported by OPSN graphs in general,
and also due to the units of practical code reuse being much smaller.

OPSN discussion, and UIs created for it,
will be a good way
to navigate, discuss, and extend this "graph of compatible open-source code" --
and perhaps a necessary way, once it gets complicated.
Fortunately, if the graph of code exists before OPSN discussion does,
it will make that easier to develop (as mentioned in the introduction above).
Also, any software tools developed specifically for navigating and extending this graph of code
should also be useful prototypes for some aspects of "OPSN discussion tools".


### it's not too much work to get started on this

The work needed to get started doing all this
(after a prototype OPSN format and protocol is defined, and a support library written)
is not too different from any work to define data formats or plugin interfaces
or scripting extensions,
but by being based on universal exprs for symbolizing datatypes and class/method/function names,
and with the use of common datatypes coordinated by OPSN discussion
(or by conventional discussion, when the number of datatypes remains small enough),
the results of much of this work can be interoperable,
since it will effectively act as
"an extension to the general OPSN types for data and code, and to the (distributed) library implementing them",
rather than just
consisting of lots of isolated formats, interfaces, and scripting extensions, as is usually the case now.

That means that as soon as a handful of people have done that sort of project together and in this new way
(within open-source projects of related functionality),
they'll be likely to see clear benefits from this interoperability
which justify the extra work of doing it this way,
in spite of the likely slower-running code and larger data size (in the short run).

