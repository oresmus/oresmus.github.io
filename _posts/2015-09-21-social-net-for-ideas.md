---
layout: post
title:  "a Social Network for Ideas"
date:   2015-09-21 17:38:38
author: "Bruce Smith"
## meta: "two week read"
tags: OPSN "semantic network" "idea graph" "global brain"
---


This post proposes a new kind of software tool and social network protocol
for people who want to engage others in **constructive discussions
about complex problems** (of all scales),
or more generally to **collaborate on complex projects**
in an open-ended way.

The goal is to increase the effectiveness of group discussions of all sizes,
by helping participants keep things organized for themselves and each other,
seeing how everything fits together,
as if they were "**seeing into a global brain**"
that they can all contribute to.
The hope (justified below) is that this could augment current large-group decision processes
so that the outcome can be more intelligent when more people are involved.

(Many parts of this proposal are not original, though I think the whole is; see the end for credits.)

## basic idea -- "Overlaid Personal Semantic Networks"

The basic idea is to visualize
the parts of the participants' **semantic networks** (graphs of related ideas)
which are **involved in the discussion**, synthesizing these into one giant graph visible to everyone,
so that existing and potential participants can see where and how each post or comment fits into the whole --
what people agree or disagree on and why,
what they think is relevant or connected and why,
who is adding interesting comments,
and how everything ties together.

This "**idea graph**" (including the complete history of public comments linked into it, plus summary views)
would be both browseable and statistically analyzable by each participant (with the help of new software tools and UI),
supporting many use cases to help them understand the prior conversation and contribute to it.

For example, if you are **considering changing your mind** from position X to Y,
the tools and UI would help you find a coarse graph-edge representing that transition ("coarse" because it's in a summary view),
attached to which you could **find the best arguments in either direction**,
where "best" means "most persuasive to the people you want to be influenced by".

(The tool would also let you open up that coarse edge into the finer-grained edges it summarizes,
seeing who had made or supported each argument, in what context, and what they thought was related to it.
The graph could have other kinds of structure as well, e.g. a general kind of directionality, described below.)

Conversely, if you want to **encourage other people to change their mind that way** (or discourage them from doing so),
you would find the ***same* coarse edge,** view the existing arguments,
then figure out what's missing and how best to add it,
perhaps adding a new argument or counterargument, or adding or removing your support for something already there,
or helping to clarify it or add evidence.

In a sense, it would be **like having a Wikipedia page for each idea** and each argument
(much finer-grained than Wikipedia itself, since anything talked about could be a topic),
except that **conflicts would be handled differently** -- no one could "remove" your edit or page,
but they could refrain from passing it on (to people they influence), or give it more or less endorsement;
whether each person sees your change depends on how it's weighted by people they want to be influenced by,
as well as on their personal preferences for combining influences and resolving (or explicitly displaying) conflicts regarding that topic.

(Handling conflicts, or more generally **combining influence from multiple sources**, is an important topic elaborated on below.
Briefly, whenever there is not consensus, there can be "**personal or factional views**" of specific items,
but these are implicitly linked together so that **alternative viewpoints can be noticed.**
The goal is to **combine the best aspects of both full individual control,
and the "single place for each discussion" property of Wikipedia.**
We want to encourage and enable consensus, but not provide an illusion of it --
the best ideas in the end might be a combination of what seemed initially like opposing views.)


## but wouldn't making this graph be too much work?

The graph would be made **mostly by the software tool itself**, guessing things from context and word content,
and partly by adjustments made by the participants,
which in most cases the tool could help them make easily.

The **participants would be motivated to help** make the graph (provided it wasn't too hard),
since, **as writers,** they want to be **influential** and properly understood,
and **as readers,** they want to keep what they've found **organized,**
and pass it on (to other readers) in a useful way.

(Note that **all the organization in Wikipedia**, e.g. the reasonable partitioning of edits into pages
(including the naming and disambiguation of those pages), **comes from exactly those motives** --
and in that case there is no classification help from the tool (beyond using it with a search engine).
We want to end up with more organization; we'll rely on the same motives combined with a smarter tool.)

### textual view (for topic tags)

More specifically, the participants would sometimes experience the tool as a "blog" or "forum",
seeing articles and chains of related comments in the usual way,
with the opportunity to add comments or make edits in that context.
The comments they see might be more or less organized, depending on what was done by their writers and prior readers.

The most basic organization is to **tag each contribution with topics**.
The topics can be as **fine-grained** as the participants like, and either **formal or informal** --
anything from a word like *"penguins"*, to a phrase like *"biotechnology-based nanomachines"*,
to a sentence like *"X is better than Y at aspect A of solving problem P"*,
to some url which formally refers to *"variable V of function F in software system S"*.

The **tool itself** can make a **good guess at useful topics,** using context and word content and user preferences,
which means the additional work needed from participants, e.g. to remove false positives and disambiguate topic phrases,
can be very small. Since that work is also optional, if the UI is well-designed it won't get in the way.
The writer can do it in a **few seconds per tag,**
but if they don't (or do it wrong), any of their **readers can do it just as easily.**

### graphical view (for connections)

The next level of organization is to introduce **links or relations between contributions.**
This might also be done in a textual UI,
but it will work best when participants are using the tool in a "graphical" UI mode,
which condenses the related portions of the idea graph
into a **summary graph** small enough to see
(by filtering and combining graph nodes and their properties),
but **retaining the most important distinctions** relevant to what they're discussing.

In this view, the user can **see the flows of reasoning** involved in the discussion, including **alternatives** --
e.g. different ways of doing a subproject, explaining a phenomenon, or arguing a point --
or **sequential chains** -- e.g. A is a subproblem of B which is a subproblem of C, or a chain of inferences.

Then they can find the **best places to put a contribution** they're writing,
or to **classify someone else's** which they're reading.
In a good UI this could all be done in a minute or two even for a new summary graph,
or in a few seconds for one they were already viewing -- for example, dragging a link-end from text into the graph
could classify that part of the text as connected to that part of the graph.
If any link-type-options were important, there could be a few checkboxes or popup menus for adjusting them.

That should be easy enough (for either writers or readers)
that **the intrinsic motivations** mentioned above -- in this case, helping people find the contribution when it's relevant --
**would get some users to actually do it;** and **it only takes one person's action** to make a given item findable in the right way.
(The tool could also enhance the motivation by providing immediate feedback,
like an estimate of near-future readership which changes when new connections are made.)

### graphical navigation

It's also useful to navigate graphically within relevant parts of the whole discussion,
in order to understand it more generally than in connection to a specific contribution.

When seeing any graphical summary view,
the user can navigate by zooming in or out, or by changing the level and nature of summarization,
which changes how the many possible inferred graph properties affect the graph layout on the screen,
or properties like color and typography.

This navigation might be done indirectly,
in the sense that commands from the user would tell the tool
to focus more or less on specific visible nodes, relation-properties, or regions,
with the tool inferring from that how to change the more fundamental parameters which generate the view,
such as an initial set of items to search from (to find the ones to display as graph nodes), a relevance filter,
and layout preferences mapping edge properties to screen offsets or angles.

What this navigation should feel like is
**moving around in a high dimensional space containing the graph** (projected onto the screen),
seeing different structure from different points of view,
using either intuitively understandable motion commands (perhaps with 3d-game-like motion control),
or higher-level commands with meanings like **"show in more detail how these summary nodes are related".**

The graph layout could also be modified in **physics-inspired** ways (already used in some current layout algorithms);
for example, to force certain classes of ideas onto opposite sides of the screen
so that their interconnections must all cross the screen.

Besides sharing edits to the graph itself,
users can also **save or share specific ways of viewing the graph.**
This allows one user's organization, in terms of connections or layout, to influence that of other users.
**Over time,** as layout parameters are combined from many such views,
**the general layout quality should improve.** (The specific layout rules,
like everything else in the graph, would be under individual control,
and spread to other users in a decentralized way due to requested influence.)

(The graphical UI is the most experimental part of this proposal.
Early versions would probably have a more textual UI, since that requires less invention to develop.
All versions would retain the textual UI modes, since they are often useful.)

### directional relations

Finally, there are inherently directional relations,
such as the one most important for understanding a project plan --
**"Y must be done before X can be started".**
Such relations also have a directional effect on how someone should think about the project --
e.g. "if you are thinking about whether X will get done on time,
one thing you should also think about (in connection with that) is whether Y will get done on time".

It turns out many relations can be viewed in this same general form:
**"if you care about X, maybe you should care about Y"**.
This gives a **single concept of directionality** to a wide variety of actual relations between ideas, topics, posts, or things,
and thus serves as an overall organizing principle (in addition to relatedness and containment).
Examples of relations with that kind of directionality include
*goal* -> *subgoal*, *whole* -> *part*, *effect* -> *cause*, *phenomenon* -> *theory*, *hypothesis* -> *evidence*.

(On the other hand, most relations should direct at least some attention in both directions,
and there are some which are mostly symmetric, e.g. *author* <-> *item* <-> *topic*.)

What is most important is that an analysis tool and UI can do a lot with that directionality,
without needing to care about the specific kind of relation it comes from.
The most glaring example is probably the **PageRank** algorithm,
in which the relation is something like "the writer of web page X thought the reader might want to browse to web page Y"
(and thus inserted a link), from which PageRank can deduce a good estimate of webpage importance.
But there are also other analyses of a directed graph which can give interesting results.
Especially, they can help us understand overall structure of possible problem solutions in terms of projects
and subprojects.

Since the choice of direction, when unifying various asymmetric relations, is not arbitrary --
it comes from how human attention should be directed --
it makes sense to apply such an analysis after unifying all directional relations into this general kind.
What this means for us as tool designers is that
**we don't need to make the human users specify the kind of relation,**
as long as they can tell us that a relation exists, and which way it should direct attention
(which is often implicit in the UI operation they use to add it).

(Of course the specific relation type can be useful, and should be used when available;
but making it optional means more relations will be entered for the tool to work with,
so it's important that its most basic usefulness survives.)

### so the work is doable and motivated

In sum,
all we need from the users is certain basic info about connections between things in the idea graph;
automated tools can guess much of this, and can then make it easy for the users to add the rest;
the users are motivated to do so, to make their writing more influential and their reading and sharing more organized.

But given this basic info, the analysis tool can organize the overall idea graph in important ways,
including project structure and hierarchy, that greatly help people communicate their ideas and the reasons behind them,
and also help them find people they want to communicate or collaborate with.
In the best cases, using the tool would feel like **"seeing into the minds"**
of both the whole group and its most interesting participants (relative to the viewer).


## how would the social network be organized?

So far I've described the idea graph that one person sees and edits, containing many people's contributions,
but I've only hinted about how different people are linked together. There is both a general question of network architecture,
and a set of **obvious problems** that need addressing -- at least including
conflict resolution, divergent ways of classifying things,
efficiently ignoring spam (so we can have a **big public network open to everyone**),
comments interesting to some people but not others, preventing spoofing of post authorship,
and ambiguity in topic phrases. (I think all of these **have solutions**, some well known and some outlined below.)

### a decentralized network

I think the best organization is decentralized, both for robustness,
and so **everyone can choose who and what they are influenced by** and how,
and what they pass on to other readers,
even though the actual combining of information
(to generate the viewing priorities which determine the small subset any one reader has time to see)
is mostly done automatically (by a tool under each reader's control).

Specifically, we can imagine that each participant is continually publishing
both a blog (with their own writing, and other posts they explicitly pass on),
and a wiki (like a personal version of a global fine-grained wikipedia,
 but only covering topics they care about, with conflicts resolved according to their own rules).

(The actual organization of the info each person publishes is more unified --
it's just their personal version of the **idea graph** described above,
published **as a stream of incremental changes** --
but thinking of it as "blog plus personal wikipedia"
makes it easier to understand the volume and nature of the information involved.)

At the same time, each user is **subscribing
to the same sorts of info published by everyone else** they want to be directly influenced by.
(Those streams include contributions from other people, who they will therefore be indirectly influenced by.
To keep volumes manageable, they can subscribe to pre-filtered streams, and offload some processing to **external hosts,
which they can "trust but verify"** to filter properly.)

Each person's own version of the software tool receives and **combines all this info according to their own preferences,**
choosing what small portion to proactively show them,
and what much larger portion to pass on (republish) (or to show them if they search for it),
including their own changes and new material.
The tool can be open source, so in principle the user can see and control exactly how it works,
and there can be independently written versions which use the same network protocol.

(Note that each person is also *not* subscribing to info from the people they *don't* want to be influenced by
(like spammers).
A reasonable reader ought to be willing to be influenced in what they see
(at least a little, indirectly) by everyone else who is also reasonable
(in part since reasonable people will not try to prevent their posts from being classified correctly,
and the reader can filter them if they don't care about those topics).
So if there is someone whom no one is willing to subscribe to, even indirectly,
we can safely consider them unreasonable. Maybe everyone is wrong and that person is just misunderstood...
but that problem exists in "real life" too, and has no perfect fix. Anyway, surely there will be a few people willing to
"subscribe to everyone" (though of course they can only view a small random sample of their info)
just in case there is something worthwhile there, which they can help rescue from obscurity.)

### hosting, filtering, and combining

There is a lot more to say about architecture and algorithms
(especially for people who are **"intrigued but skeptical"**
about whether this scheme could possibly work);
but to keep this post from becoming even longer,
I've put some of it into a separate post

* [OPSN network details]({% post_url 2015-09-21-OPSN-network-details %})

which contains the following subsections:

* **hosting companies and ads** --
how an open-source decentralized protocol might coexist with multiple profit-making hosting companies
(and what to do if it turns out it can't);

* **spam and other uselessness** --
how filtering based on a **"chain of respect"** can give spam the low influence
(and consequent invisibility) it deserves;

* **conflicts, viewpoints, and ambiguity** --
handling disagreements constructively, so **chaos helps coherence evolve** rather than destroying it;

* **dealing with "echo chambers"** --
they'll exist, but you can notice them and look outside if you like.

(There are also things I'm leaving out since I hope they're sufficiently obvious,
like the possibility of private overlays to the public global semantic network,
and issues related to multiple or pseudonymous identities
(unavoidable and ok -- note that no one is forced to pass on their posts)
and cryptographically signing posts to make them unfakeable (desirable and well-known).)

## how could this start and spread?

A good-enough version of the proposed tools and UI would be **useful even for one person**
who wanted to organize their ideas and work, or what they had read.
(It would be especially useful if it was integrated
with their other tools for working on complex projects with interrelated design decisions,
such as IDEs for software development.)

If their own ideas, work, or reading related to something they wanted to discuss with others,
they'd like it if those others were using a compatible tool, so the discussion could benefit
from this organization and visibility of its complex aspects.

So if good enough (and free),
a tool like this (or compatible features added to existing development or collaboration tools)
**could spread virally,
as people tried to get their collaborators and discussion partners to use it,**
or decided to host their blogs in this new way,
or used it as an adjunct to existing blogs which they were writing or reading,
or to complex projects they were already collaborating on.

At an early stage, the new tool and network might contain more pointers to external writing than its own hosted writing,
but it would be almost as useful when used that way.
(It could contain both hosted writing and external links indefinitely.)

The new network **might initially spread though niche communities** with special interests and UI needs,
which its new features make the biggest difference for
(such as discussions about **design decisions in large software projects**),
or who have a special interest in it
(such as the people interested in new collaboration and discussion tools).
Being open source, it is also likely to be customized for those needs,
for example gaining native ability to view and edit **specialized datatypes like math or code,**
or specialized kinds of metainfo on the idea graph (such as results of running code that appears in the graph) --
which is fine as long as there is a **unified network protocol, so variants can remain compatible.**

(Of course we'd like to let new datatypes be added both compatibly and in a decentralized way, and I think that can be done,
but discussing how would be too big a digression for this initial post.)

### how far could it go?

If someone wants to record a "sufficiently notable fact" today,
they'd ideally like it to end up in Wikipedia,
since that's the first place people look for such things.

More generally, for most things anyone puts on the web,
they would like them to be found by a relevant Google search,
for the same reason.

A system like the one proposed here could reach the same status,
for public ideas and reasoning about complex or important topics.
As soon as it worked well enough, 
its community of users and set of covered topics could grow --
at first just by serving the community doing the discussing, but later,
once it became a good source to look to
(even just for limited topics),
for the extra reason that
people would want to make sure it contained their own preferred ideas,
and the reasoning they thought was most convincing.

Eventually (after much evolution and improvement),
such a system seems likely to contain the best intelligent public discussion about most topics.
(That's why it's so important for it to be open and decentralized.)


## what good could this do? 

Since prehistoric times (as far as I know),
small groups of people, under the right circumstances,
have been able to discuss a problem and come to a better solution than any of them could have individually,
provided some of them have the necessary knowledge and intelligence,
and enough of them the necessary wisdom to appreciate those.

### "open discussion" works for small groups ...

The "discussion process" used by a small group goes something like this:
everyone keeps the others updated about their evolving take on the part of the problem they understand;
at the same time, everyone uses their unique individual wisdom,
and their personal knowledge of the other people,
to evaluate and integrate the others' views into their own.
Eventually (depending on the kind of leadership or anarchy in the group)
a consensus is reached, or the recognized leaders make a decision,
or some of the individuals decide what they personally will do.

(If we imagine each person's ideas as a semantic network, then as this process proceeds,
the related parts of those networks change and expand so as to have a sufficiently similar structure
to count as a "common understanding". This doesn't imply they agree,
but that they understand the problem in compatible terms they can use to communicate.)

### ... but is not enough for large groups facing important problems

Groups that are too large for that process to work well, like companies or countries,
still often try to come to a good group decision using many people's input.
We might hope that the increased knowledge and intelligence, due to more people being involved,
would make the result even better, but this happens depressingly rarely.
Unfortunately there are many problems looming,
that very large groups (like countries or the world) need good solutions for in the forseeable future.
Some of these are complex and difficult enough that surely we need the input of many different people and points of view,
and any good solution is likely to be a synthesis of their ideas which no one can now forsee.

(For some of these problems it would help a lot to allow more freedom to individuals or local groups,
but for many, some kind of whole-group decision will be needed --
one example being the question of *which* problems local groups and individuals should get more freedom to solve for themselves.)

### OPSN can bring the benefits of small-group discussion to any scale ...

The proposal of Overlaid Personal Semantic Networks (or OPSN --
by which I mean all the elements discussed in this post, taken together;
not just the "idea graph" data structure which that term directly describes)
is just an attempt to take that open discussion process, that works well for a small group,
and imitate it (using new technology) in a way that can work
for a much larger group (everyone who wants to participate).

It does that by making the important shared parts of the participants' semantic networks visible,
so each conversation between a pair or small group of them,
which leads to a common understanding (even if not to an agreement) within that group,
also leads to a visible form of that common understanding in the public network,
which other participants can later join into, build upon, or explicitly disagree with.

We can compare OPSN in a large group discussion to the native human ability to hold a small group discussion:

It preserves the role of each individual human as the only thing that can intelligently evaluate and integrate 
other peoples' contributions to the group discussion.

It doesn't overcome the need for each individual to form opinions over time 
of the ideas of specific other sources (people or subgroups) on specific topics,
nor the fact that each person can only evaluate a limited number of those.

The World Wide Web
has already freed people involved in the "global discussion" from
the constraints of physical proximity,
both for who they network with and how they allocate personal time,
meaning a large network containing everyone is possible
(even if each person only connects directly to a few others).
(You could say the same about the printing press; but the Web makes publishing so much easier and faster
that it has qualitatively different effects.)

But a discussion architecture and tools like those proposed here, augmenting the Web,
can make the whole discussion-system much more effective in several ways:

- it can preserve much of a discussion (not just words, but context and connections and reasons),
and keep it connected to whatever issues, topics, and other discussions it's relevant to,
so new people can much later find just the right one, and contribute to it;

- the sources one person is evaluating can not only be individuals or single posts, but synthesized views of groups;

- the passing on by one person of most of the views of another person he or she respects (on certain topics) can be largely automated.

### ... with profound effects

One overall effect is that the amount of information coming from each person is vastly more than one person could transmit by speech
or their own writing,
but it has still been pieced together from other sources, and organized,
according to that person's unique judgement of their quality regarding specific topics.

According to the fascinating book *Edmund Burke: The First Conservative*, Edmund Burke invented the "political party", 
which allowed people to vote for sets of ideas rather than just for individuals, 
providing a system in which sets of ideas (meant to be comprehensive enough for governing) could evolve over time
to be more attractive to voters (while being periodically tested in practice).

A related overall effect of OPSN is that the information stream coming from each source (curated by a single person or group)
can be as comprehensive as a political party's ideas --
but we can have as many of these comprehensive systems of ideas, evolving, competing, and being synthesized into better combinations,
as there are people in the network. The same goes for integrated sets of ideas about other topics (unrelated to politics),
even if they are only of interest to a small fraction of people.
(This is an important advantage of the network architecture in which each person is effectively
publishing their own continuously updated "fine-grained version of Wikipedia". Unlike in the real Wikipedia,
there will be no prohibition against "original research", and what is notable enough to include is up to each person.
Any filtering about that can be applied by each person subscribing to that version, perhaps differently by each one.)

This means that any given person, combining and improving idea-streams curated by people they respect, 
can produce something whose quality is slightly better than the average quality of their inputs 
(at least when the input quality is comparable to that of one human's set of published ideas) --
in fact, they will do so automatically, just by browsing and organizing the combined inputs for their own use,
and allowing the effects of that to be republished --
and therefore, after people have been selecting the best new input streams they can find for some time,
the overall system will produce some idea streams
of much higher quality than whatever could be produced solely by one person.

These higher-quality personalized coherent sets of ideas would
not replace any of our existing decisionmaking processes,
but could augment them
by serving as new input for the people making the decisions -- which is all of us.


#### **credits** and related work

I am indebted to many sources and existing systems for inspiration and specific ideas.

This whole proposal can be viewed as just a specific variation of "hypertext",
as envisioned by Vannevar Bush, Douglas Engelbart, Ted Nelson, and Eric Drexler.
The things it might add or change include network architecture, desired metastructure of hyperlinks,
proposed UI, and mechanisms for user motivation; but the broad goals and predicted benefits are largely the same.
(But I am not certain of any specific part being original, since I have not read every relevant thing by those authors.)

I also want to credit the ideas of Tim Berners-Lee (not all of which are yet embodied in the WWW);
the WWW, Google/PageRank, the wiki, Wikipedia, blogs;
source code control systems, especially distributed ones;
Google Wave; the Diaspora project.
(There are probably others I don't remember.)
(If I went into more detail about implementation, I'd add at least the Merkle tree and [IPFS](http://ipfs.io/) to this list.)

Though many parts of this proposal come from those sources and others, as far as I know this proposal as a whole is original;
at least I have not found another one which contains all the elements
I describe here as needed in one system,
for it to potentially succeed in the ways I describe.
But if I missed something, I would love to find out about it in the comments,
or for it to be added to the [collaborative design wiki](https://github.com/oresmus/collaborative-design-wiki/wiki).

I have also discovered other people's projects with related goals,
including some a few days ago when I first googled "idea graph" after using that term in this writing;
but most people using that term are referring to what I would more likely call "the common-topic graph",
so for them it has thousands of nodes, rather than billions. (The one or two most-related projects I found seemed to be defunct
and to have very little description available.)

I think there are several projects described as building and using a
"topic graph" or "knowledge graph" (including one already used in Google search results), but as far as I can tell,
they are trying to make a graph of common knowledge (automatically and centrally)
rather than one containing every contributor's individual opinions and reasoning (by user participation, decentrally).

#### thanks

I am grateful to my friends who have read and commented on earlier drafts
(in some cases an embarrassingly long time ago),
whom I thank for their reading and feedback,
especially Eric Drexler, Peter McCluskey, and Erik Messick, who provided the most extensive comments,
and also John Baez, John Michelsen, Mark Sims, Will Ware, and probably some other people to whom I apologize for forgetting.

