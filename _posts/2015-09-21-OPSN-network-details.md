---
layout: post
title:  "OPSN network details"
listed: false
date:   2015-09-21 17:34:44
author: "Bruce Smith"
tags: OPSN "semantic network" "idea graph" "global brain"
---

To save space, I removed the following subsections from the main post about Overlaid Personal Semantic Networks (OPSN),
[a Social Network for Ideas]({% post_url 2015-09-21-social-net-for-ideas %}),
which now points to them from its section on *hosting, filtering, and combining*.


### what about hosting companies and ads?

Of course most people might choose to let a company host the OPSN-network-filtering-tool for them,
even if its version was closed source,
both to avoid having to install it,
and since it requires a lot of network and CPU power to run,
a lot of which might be avoided (by exploiting shared work)
when running many instances (for different users) together.

But there is no need for this to be the same company for everyone,
and indeed different companies might cater to different niches within the general set of people who want to participate.
Having a network architecture which is fully decentralized in principle
reduces barriers to entry for specialized companies, even if it's not fully decentralized in practice --
competition between hosting companies can be more about "value added" than pure network effects.
It also preserves the ability of individuals to host and experiment with their own version of the software tool
(by itself, or connected with instances run for them by hosting companies),
while remaining connected to the global network.
(Note that most of this can be said about web servers as well,
and that was an essential factor in the web becoming popular.)

At the same time, a distributed network doesn't preclude profit-making companies which either charge for the hosting service,
or insert ads into the stream of info a person receives.

Ads would ideally be marked as such
(by coming from virtual "authors" whose profile metainfo labelled them as sources of ads).
That is not enforceable technically, but it is socially,
since ads and their sources are subject to the same feedback mechanisms as regular posts,
which will make disrespected posts almost invisible;
and since there are well known cryptographic mechanisms to prevent sources from being spoofed.

(Indeed, even a regular human user who mainly posts ad-like material
would soon find they were either not chosen as an influencer by most people,
or were marked as mainly providing ad-like material (with the consequent changes to their posts' visibility)
by the people who did accept their influence.
The same would go for any other mark that authors ought to put on deserving posts themselves -- if they didn't,
the collective action of recipients would soon either reject them or add the mark themselves.
This doesn't even require a new feature --
marks warning about certain kinds of content are just another kind of "topic tag".)

Note that ads could potentially be well targeted and high quality,
since users would be expressing their interests directly and semi-formally
(in a sense, that's the whole point of the network).
So even though users would have the power to turn off ads completely,
it's quite possible many of them would allow some ads through,
and some of them would provide constructive feedback on those of borderline quality.

(There are also potential ways hosting companies could profit from network analysis,
unrelated to serving ads. Since the data is public, the hosting company's advantage is just more efficient access,
so there is no special privacy concern regarding a hosting company --
they could have read it anyway even if they were not hosting it.
Of course this is not true for data in a "private overlay network",
but for that the user would presumably pay for hosting and have a contract spelling out allowed uses.)

### spam and other uselessness

You would only ask to be influenced by someone you thought was worth listening to,
and trustworthy enough to try not to pass you anything illegal;
and you can make that influence topic-specific if you like, and weight it however you like.

That means each item your OPSN tool receives only got to you along a **"chain of respect"**
(in which each person felt that way about the person they got it from);
and the tool only proactively shows you the items which have a sufficient weight,
according to your weighting rules (which can use the weights applied by each prior person in the chain).

If you do see spam from one of your friends (unwittingly republished by them from some other source),
you would tell them, and either they would cut off the ultimate source from their feed,
or you would lower the weight of "items like that", or if that was impractical,
you'd lower the weight of all items reaching you only via that friend.
This would lower your chance of either seeing such items or passing them on.
(You could also adjust weights differently for those two actions if you liked.)

A bigger problem is something that the poster honestly thinks is useful, and maybe so do many of the people passing it on,
but you don't. Some of this is unavoidable, but in principle the idea graph can help a lot.
For example, if the reason it's not useful to you is the assumptions it makes or the problems it addresses,
no one ought to object to people classifying it correctly in those ways,
even if you view those classifications as making it less attractive and they think the opposite.
So if enough people think those classifications themselves are useful, they will probably occur.
The more popular the item is, the more this matters, but also the more people can share the work of classifying it.


### conflicts, viewpoints, and ambiguity

Sometimes people agree something is useful but not on how to classify it.
Therefore you would often receive the same item from multiple people, but classified differently.
(Your system would know it was the same item, since all items -- even single edits -- would have a world-unique id.
The classification would not affect the id. Techniques for efficiently assigning ids like that are simple and well-known.)

Your system would combine this info
in light of the influence-weights you assign to each person you get the item from,
which would often be topic-specific,
also using whatever weight that person had assigned the item. It would do this automatically --
no per-item work would be needed by you except for the items or topic-sources you wanted to manually rate.

Whatever your system showed you (about how to classify a given item),
if you thought it was wrong you could reclassify the item yourself
(either directly, or implicitly, by moving it around in your graph layout or other organization),
which would affect both how you saw it later (which is the main reason you'd bother to do it),
and what you passed on to your readers. (The change would normally also be visible to the people you got it from, if they cared.)

If you want, your UI can show you an indication of the level of disagreement about each tag on each item,
and about each way items are connected.

So if a community wants to converge on a common classification and organization, 
or use disagreement as indicating a need to clarify something, they can.

Or they might split an item in two, making one new item for each way the original was being classified.
For example, this might happen if some suggestion could be interpreted equally well in two different ways, both useful;
or if some new term had more than one incompatible useful definition;
or if a piece of computer code came to have more than one version, each one better for certain uses.
Once someone noticed that people were arguing about which version of the item was "correct",
then rather than continue the argument,
they could just **"fork" the item** (to use the term from distributed source code control systems),
making clear on each new variant how it resolves the ambiguity about "what's best";
then people can use each new item in the appropriate places
(and tag the old one as "deprecated due to ambiguity" to encourage its uses to be replaced with one of the new ones).

This also applies to topic phrases and their meanings. The scheme of splitting what's ambiguous into more specific versions
has worked well for making and naming Wikipedia disambiguation pages, and should work well for this system too.
Though this system has far more topics in all,
each finer-grained topic is interesting to fewer people,
so no single person has to deal with too many topics for this to be practical.

Edit conflicts (e.g. different preferred phrasings within one item) are harder to handle,
but the same general idea of voting among your feeds,
seeing the level of conflict if you wish,
and communities trying to converge when they can, or to split one item into several otherwise,
should work, whenever enough people want it to work.

Wikipedia forces this to *always* work, essentially by letting the version with the most active-user-support win,
whether for one item's text or how it's classified or even whether it can continue to exist;
but this has the cost of sometimes hiding the views of minorities
(at least that has been alleged, and is easily imaginable in theory -- I have no direct experience of it).

The presently proposed system can't fully hide anything, in principle.
If most people think a car has tires, but you think it has tyres,
you can republish the "tyre version" of the relevant item even if none of your feeds provides it,
and what your readers see depends only on how they weight your influence compared to other people who supply them the same item.
Readers who like to see disagreements explicitly (to help them understand the conflict and do something to improve it --
in this case to recognize the country-specific spelling and disambiguate it)
might see both versions, and a numerical weighting for each, which is impossible with Wikipedia
(unless you delve into the edit history).
(They could even have a way to see what each version is correlated with, to help them understand the nature of the conflict.)


### but wouldn't these features support "echo chambers"?

Yes, for people who want them, but they exist anyway.
(In fact, even a single-consensus system like Wikipedia in some sense supports one big echo chamber,
which is ok when the consensus view is correct (as it often is),
but dangerous when it's not.)

Our target audience is more likely to want diverse inputs,
and to want to see explicit indicators of significant levels of dissent,
since they want to avoid fooling themselves
with the illusion of consensus being stronger than it actually is,
and also since they want to see *why* someone disagrees (or agrees) when they do.

The presently proposed system allows anyone inside an "echo chamber" to notice this (if they want to),
and makes it entirely an individual choice whether to go along with that or "look outside".
(To implement "looking outside", assuming one of your feeds is providing the disagreeing items at any weight,
let your statistical/graphical analysis software detect the echo chamber in the complete ideagraph-portion you have,
as a dominant cluster with respect to random browsing,
and show you the highest-rated items not inside it.)


#### possible updates

If people ask about other network details I left out, I might add them here.

For example:

* It's desirable to have automatic and machine readable licensing for contributions;
the default license should allow them to be copied and saved, and rehosted from elsewhere,
and give everyone the right to comment on them, and to use them in certain ways.

* The protocol should provide a way to request the removal of an item, by giving a reason you want it removed,
which each host can evaluate and react to as they wish.
(Any stronger requirement is not technically enforceable, though of course it might be legally enforceable.)
In fact, in some jurisdictions this is essentially required
(e.g. every US host must provide a way to receive a DMCA request, or risk excessive legal liability).
(Technically, a removal request might just be a special tag.)

[update 10/9/2015: I'm nearing completion of a followup post
which goes into details of possible data formats and network protocols,
especially concerning how they support decentralized extension of general data formats and relation types.]
