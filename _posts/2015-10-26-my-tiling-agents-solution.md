---
# -*- coding: utf-8 -*-
layout: post
title:  "solution to Tiling Agents problem"
date:   2015-10-26 23:53:15
listed: false
author: "Bruce Smith"
tags: AI logic
---

This is a brief description of my solution to the "Tiling Agents" problem [YH2013]
(about using formal logic for safety rules to govern general-purpose machine intelligence).

(It elaborates some emails I sent to a few potentially-interested people around February 2014.)

This is a draft, and could use some better organization, condensing, and perhaps more background information.
For now, I am not trying very hard to make this post understandable or interesting
except to someone already familiar with the Tiling Agents problem and with certain related topics.
It also has one or more xxx's marking places which need fixing.

[Update 1, 10/27: fixed typos.
Update 2: added reference [YH2013],
and discussion (at end) of how this solution differs from related "failed solutions" discussed there
(also other minor changes).
Update 3, 10/28: extended generality discussion.]


### summary of solution (for people who already know the problem)

The "Löbstacle" arises when you try to restrict a machine's actions, for safety,
to those it can prove have a safe result;
but then, for flexibility, you want it to be able to create agents like itself
and delegate some or all of its job to them.
The problem is that it can't prove this action (delegating power to the agent) is ok,
even if the agent is identical to itself (in which case *we* see that it's obviously ok),
since this would amount to proving itself ok, which it can't know while remaining consistent.

My proposed solution (to the "Tiling Agents" problem --
it's not a solution to the Löbstacle, but rather a way to avoid needing to solve it)
is just to explicitly allow a machine to make a new (or recognize an already-existing) "high-level copy" of itself,
as an acceptable agent to give power to.
By a "high-level copy" I mean an arbitrary but provably correct implementation of the same high-level program
the original machine was an implementation of.

In other words,
instead of just one way to prove an action is permitted (namely, prove the action's result in the world is ok),
the machine gets two ways: prove the action's result is ok,
or prove its effect is to delegate powers to a sufficiently-equivalent machine.

(One complexity of this idea might be in formalizing "provably correct implementation of the same high-level program".
I know how to do that, and consider it simple and perhaps obvious
(given that the goals of the theory are purely logical rather than practical),
but I haven't fully written it down,
and I don't know whether it will be considered either obvious or well-known by anyone else.
In any case, for the proposed solution in this post to make sense,
all we need is to assume a theory like that
*exists* (and is capable of formalizing that condition),
not for it to be described or understood here.)

It's easy to show that this new kind of machine is safe,
provided we assume the original kind (which has only the first way of proving an action is permitted)
is safe -- which we do assume, but which is not itself provable,
since this amounts to assuming
that any action that can be proven safe (and therefore, any action the machine does) is actually safe.

I also claim, about this solution:

* it's as general and flexible as it needs to be,
to handle agents which are "improved in any reasonable way".
(That is an informal claim whose main argument consists of the challenge
"think of something legitimate which this scheme couldn't handle".)

* it also cleanly solves the related issue of
"noticing your own physical manifestation as if from outside,
and not feeling you need to stop it (due to it unsafely using or interfering with your own powers)".

This solution is related to one "failed solution" and one "missing solution"
discussed in Section 5 of [YH2013]. At the end, I discuss how it differs from those.


## background and problem


The background situation is this:

We imagine that at some future date we will be able to construct generally intelligent machines
and want them to help solve certain problems
using *physical* powers which could be very dangerous if misapplied.

On the one hand we might want to give them freedom to operate in a general manner,
so as not to severely constrain their usefulness.
This makes it difficult to avoid giving them access to a dangerous level of physical power --
after all, 
even the power to "connect to the internet"
is widely understood today to be highly dangerous when abused, as is fairly routinely demonstrated by humans.

But it's not practical to review everything they do before they do it --
even if we carefully review their general plans (as we obviously should),
controlling the machines carrying out the plans might also benefit greatly from general machine intelligence
applied in realtime,
but we obviously can't review all actions about to be taken by those controllers --
nor could we quickly evaluate them for safety, even when we could review them.

So we need to be able to give the machines "safety rules"
which they prioritize higher than anything else
(certainly higher than the completion of their "missions", and higher than avoiding
destruction or damage to themselves).

There are a lot of problems of many kinds to be solved before anything like that could wisely be attempted.
(For example, no one has much of a practical clue how to even *formulate* the kind of safety rules we actually need.)

**I don't think those problems are close to being solved,** and I am not advocating taking any shortcuts.
For perspective,
**completely solving the current problems of computer and internet security would be *much easier***
than solving the necessary problems for safe deployment of general AI
(in fact it's reasonable to treat solving those as a necessary precondition) --
but we're apparently not close to solving those either (in a practical sense).

But this post (and the "Tiling Agents" problem) focuses only on one narrow aspect of that general issue
(which is necessary but far from sufficent to solve),
which is how a generally intelligent machine,
able to physically influence the operation of other such machines
(by constructing new ones or altering existing ones)
and possibly able to physically interfere with itself in undesigned ways,
could correctly apply formal logic
to evaluating such actions in light of strict safety rules.


### the "Tiling Agents" problem (background situation)

(This problem is by Eliezer Yudkowsky. I have read some of his work on it
(and some posts, by him and others, on a related private email list)
but I'm not referring back to that while formalizing the problem here,
so this formalization may not be exactly like his. But as far as I know,
it is "equivalent in spirit", and close enough that any good solution to one should translate to the other.)

The Tiling Agents problem idealizes most aspects of the background situation described above,
in order to focus on its pure logical core.
For example,
it assumes a physical world in which machines can be constructed (or interfered with) by other machines,
but whose physical laws are so perfectly understood
that logical theorems about the consequences of all necessary physical actions
(whether purposeful (by the machines) or "natural" (due to the passage of time))
can be formulated in a useful way, can be formally proved (as a precondition of the machines taking purposeful actions),
and can be trusted to be correct.

(That is not just an assumption about physics and machine designs,
but assumes we have software technology for making and using formal proofs quite a bit beyond what we have today.
In current practice it is a big news event when a tiny operating system is formally proved to have an important property.
Proving interesting things (like an absence of security bugs)
about large (but typical) software would be extremely valuable,
but is never done in practice since no one yet knows how,
at least in a practical enough way for the necessary work to be supported.
But that is arguably a technical issue rather than a logical one, so in this problem we're assuming it's solved.)

Specifically, we consider designing a machine to try to accomplish some purpose,
while certainly and provably meeting a higher-priority safety goal,
in a fully-understood closed world (which will be even further simplified than the following, in later sections);
for the purposes of this problem we assume:

* there is some portion of the real world which we can consider as an isolated, fully understood "closed world"
(from now on we'll just call that closed world-portion "the world");

* the physical law of that world (at some useful level)
is fully understood (not only by the machines, but by we humans considering the problem),
and can be approximated as either "discrete" or "discretely describable" for the purposes of reasoning
about machine operation and actions;

* that physical law is sufficiently regular and deterministic to allow designed, programmable, general-purpose
machines to exist in that world (though it needn't be fully deterministic);

* the initial physical state of that world falls within understood reasonable constraints (for example,
these constraints will imply that it doesn't (initially) contain a bunch
of machines even more intelligent and powerful than ours, which are going to try to interfere with ours (unless, perhaps,
our machine is going to be in an "absolutely controlling" position/situation with respect to them));

* we can specify
enough of the initial physical state to describe the existence of at least one properly initialized
general-purpose intelligent machine of our own design (but of a limited size)
(which starts out in a reasonable situation in its environment,
which (its situation) also satisfies a formal constraint we understand and can program the machine to understand);

* but our ability to specify the initial state is *only* enough to do that -- that is, what really happens is that something
external gives us a "tame" but otherwise arbitrary (and unknown to us) initial world-state,
which 
we are then allowed to alter just enough to insert our machine into,
but which then evolves according to its own laws without our being able to affect it any more
(not even to communicate with the machine we inserted);

* the safety goal (for the final world-state) might be described either as
"cause this world to achieve a state satisfying some predicate P within some time limit T",
or "... at the almost-exact time T",
where we ought to be able to solve either form of the problem, as long as the time limit is not too short,
and for any goal picked from a specified reasonable set;

* we can think as long as we want to design our machine (including its internal programming),
but we only get to know the physical law, the machine's goal, and all the other formal constraints and problem parameters,
not the initial world-state itself.

Then the "background problem" is this: something creates a world like this,
inserts our machine into it, and sees whether the world state meets predicate P by or at time T.
But to "solve" the problem, not only must our machine always meet that goal (the safety-predicate P),
but we must be able to **formally prove that our strategy** for making machines given the problem parameters
**always designs a machine which always meets the specified safety-goal.**

But that problem is still just the *background* for the Tiling Agents problem, which is described in the next sections.


### basic architecture assumption about a "general purpose intelligent machine"

The basic architecture assumed for a "general purpose intelligent machine"
(for the purposes of this problem)
is that of a machine which has some ability to:

* maintain a formal model of itself in its environment
(describing both itself and the environment in sufficient detail for the purposes here),
updating it as needed due to sensations and (perhaps also) its own actions;

* internally propose and consider actions which it can describe in terms of alterations to that model,

* evaluate those actions (accompanied by whatever other actions might occur in parallel due to other machines or "nature")
based on their predicted effects (using a correct theory, so it's never wrong
when it thinks (i.e. has formally proven) that some action would lead to a state meeting some predicate),

And which also has:

* safety rules which constrain it to only take actions which it has proven are "ok" according to some rule we'll
need to design;

* some mechanism to propose, evaluate,
and actually take (as permitted by the safety rule) sufficient actions to (sufficiently often) accomplish some "useful purpose".

Since we're concerned about safety rather than efficacy in this problem,
we don't require the formal goal P discussed earlier to cover the "useful purpose" the machine
is trying to accomplish, nor do we require the machine to always succeed in that purpose
(let alone require it or us to prove it always succeeds).

Instead, the only thing we try to capture in the formal goal P is that "world-safety hasn't been violated".
So, if we succeed in the "background problem", our machine is provably safe, but it's an open question how often it's effective.
(A machine which does nothing would be provably safe, for most reasonable formulations of the world-safety goal.)

### a machine should be allowed to delegate to an agent ...

For a wide range of "purposes", a useful strategy of a machine is to make another machine (an **"agent"**)
to help achieve the purpose.
(Alternatively it could just grow bigger or more powerful,
but when that is considered in detail
it has the same logical problems as making an agent
(it might even be thought of as the same situation
described differently),
so for simplicity we just concentrate on the machine's needing to make an agent.
Another related problem is "self-improvement",
but this could be achieved by
"make an agent better than yourself, giving it all your knowledge, resources, and responsibilities (goals and rules),
and after that don't bother it",
so logically it reduces to the same problem.)

So at a high level, the "Tiling Agents problem" is just: make sure that a machine following a safety rule
is allowed (by that rule) to make any agent, and delegate any powers to it, which might help achieve its purpose,
in any situation and manner which we (hypothetical watching humans)
can prove would be safe (or "feel like we ought to be able to prove safe",
since part of the problem is just making sure we actually *can* prove that,
in situations where we "obviously should be able to").
(Whether making an agent is a good idea in a particular case we leave up to the machine.)

### ... and should be allowed to do anything it can prove is safe ...

If a machine is going to be generally intelligent and take advantage of that,
it might come up with better ways of achieving a purpose
than any we can anticipate and program into it
(in fact, that might be a main point of our making it,
though access to places we can't reach or physical powers we don't have could be another reason).

So we want to let it come up with new things to do, in some general way not discussed here
(since it concerns efficacy but not safety).
On the other hand, we don't want it to do anything unsafe.

The compromise is to make it formally prove
that anything it's about to do is safe, before doing it.
(Or more generally, we require it to prove that what it's proposing to do is *allowed* or *ok*
(conditions we get to formalize, related but not necessarily identical to *being safe*),
and *we* should then prove that it following this rule means whatever it does is safe.)

To recap: we specify some "action-is-allowed-predicate" about an action
(or more technically, about a *formal description of an internal action-command*, also parametrized by a
*formal description of current state of self and environment*),
which says whether that action is "ok" (to take, in that situation),
and then build the machine so that, if it wants to do some action to help achieve its purpose,
it first tries to prove that action would be ok,
and can only do the action after having proven that.
(*Which* action it wants to do, whether it's smart enough to prove it ok quickly enough, whether it still wants to do it
after proving it can, etc,
are all up to its "intelligence", only affect its efficacy (not its safety),
and thus don't concern us here.)

(As said above, the numerous hard technical problems in doing this in practice
are taken for granted in this scenario,
which is just concerned about the logical form and use of a general but correct safety rule,
so that if our initial machine provably follows it, then this provably leads to the whole world reaching a "safe state"
(one which meets safety-predicate P) by or at time T.)

### ... but it can't prove delegating to an agent is safe!

This problem is known as the "Löbstacle",
and is familiar to my initial intended audience (so is mostly not described here).
Nonetheless I'll
explain it in more detail below,
but first we'd benefit from a simpler and more precise formalization of the whole situation.

### simpler formalization (re time and multiple machines)

The careful reader will have noticed a certain amount of imprecise language in the preceding description
concerning actions, states, state-predicates vs. time, other machines, etc. To make things simpler and more precise,
we'll further simplify the modelled situation
in ways which don't affect its "logical core".
(I won't be *proving* they don't affect it --
I can't rule out that this actually makes our problem easier, though I *think* it only does so in practical ways
rather than logically -- but certainly this can't make it *harder*, and the "Löbstacle"
is still an issue in the simplified version to follow.)

First, we don't want our machines to have to "hurry", due to each other or to the natural world.
So as long as any machines exist,
we assume the world is static except for internal state changes in machines (including sensing or thinking),
and machine actions (which affect the machine taking the action, plus
any amount of "affected stuff" in the world, perhaps including parts of the same or other machines
affected in ways other than their normal operation (e.g. of sensors)).
(Only if there are no machines do we allow the world outside of machines to be dynamic.
Effectively this means that anything dynamic must be modelled as "part of" some machine which is responsible for controlling it,
except possibly when there are no machines left at the end.)

Second, whenever there is more than one machine,
we'll consider them all part of one "composite machine"
which ought to have been designed as a whole, and which we'll analyze as a whole.
And when that "composite machine" changes in some structural way
(i.e. other than as part of its "usual internal operation", which our model of its design covers --
e.g. a new individual machine is made and becomes part of it),
we'll model that as the initial composite machine replacing itself
with a completely new (albeit mostly similar) composite machine.

(This is not a "loss of generality" in what can happen -- only in how we describe it, for this analysis.
Logically, it just means replacing an action-description
"create an agent, and delegate some powers and responsibilities to it"
with a different one (for the same physical event)
"create a fancier agent, delegate all powers and responsibilities to it, and at the same time disappear".)

Finally, to avoid having to formalize maintaining the relation between one (composite) machine M's
internal world-model and the world, as M acts on the world multiple times in succession,
we'll condense everything M does before disappearing into one (perhaps very complex) action.

This means every machine M1,
after being created in some state
(either initially, or as an effect of a prior machine's action -- in the latter case,
M1's state *is* allowed to be correlated with the state of its environment,
since the action which created it can cause that),
has the following complete lifecycle:

* do any number of internal state changes;

* then disappear
(leaving behind a bunch of "machine parts" which, of course, continue to follow the same physical laws they did before --
if this means any of them are still changing, we have to be careful to follow the rule above about dynamic stuff being
considered part of a composite machine as long as one exists),
at the same time as doing **exactly one action:**
  * which may or may not create a new (composite) machine M2
  (out of specified physical stuff, some of which can overlap with the left-behind machine parts from M1),
  * and which may do something to the rest of the world.

The point of this simpler structure is to let us **model the world-history as a linear sequence of states**
which can be understood as a linear sequence of single-machine-lifetimes
(each compromising a succession of states, in which all the non-static stuff is part of the composite machine)
followed by a world-with-no-machines (also a succession of states, which is allowed to include dynamic stuff).
Furthermore, each machine's sole and final action consists either of delegating all power and responsibility
to an "agent", or doing some other action on non-machine parts of the world (or both).

All this allows us to restate the "version 1" safety rule (described earlier),
which covers a general non-agent-making action (which in this model leaves the world with no machines in it),
but not an agent-making action, as:

* M's action must provably leave the world in a state which will evolve (by the world's known physical laws)
so that it meets safety-predicate P by or at time T.

(Recall that P and T (and the choice of "by" vs "at")
were parameters of the "background problem" described above.)

(We assume that the initially given world-state must also satisfy this, so that creating an "empty machine"
(one which comprises none of the physical stuff in the world)
would be safe. [xxx i should say this earlier])


### the "Löbstacle", more precisely

The above model is unsatisfactory in two ways:

* it has two kinds of actions, which differ only in terminology -- since a machine, being made of physical stuff,
could just be described as that same physical stuff instead, which means an agent-making action
could alternatively be described as a non-agent-making action which just leaves the same machine-parts doing the same things
in the same place in the world, but describes them as "non-machine stuff" rather than as "the parts of a new machine";

* it only comes with a safety rule which covers the non-agent-making actions.

On the other hand, as long as we design a machine which never uses an agent-making action,
we can (if we design it properly to use that safety rule) prove that machine is safe
under our standard assumptions (which include the soundness of the proofs of action-permissibility we require the machine to make).
The argument is trivial (since the details unimportant to the present topic are being glossed over, though correctly so):

* the machine operates correctly (since we designed it that way, the world has the laws we expect, etc);

* therefore, it only takes actions which satisfy its safety rule (and by the present additional assumption of this section,
which are non-agent-making actions);

* i.e. it only takes actions which leave the world in a state S for which there is a proof that S will lead to P by or at time T
(in short, for which S is a "safe state" when treated as a non-machine state);

* it eventually takes such an action (by another assumption we made earlier);

* since the proof system it uses is sound (we assumed), it's *true* that it only leaves the world in a safe state.

The two unsatisfactory aspects of the above model seem complementary -- we have no safety rule for agent-making actions,
but all such actions could have been considered as "standard" (aka "dying but stuff-affecting") actions anyway,
for which we have a provably-good-enough safety rule. So can't we just dispense with agent-making actions
and treat everything as a standard action using the version 1 safety rule?

We could, but when we try this with a sufficiently capable agent
(for example, a copy of the original machine), the "Löbstacle" shows up, because:

* you can't prove it's ok to "do any action you can prove is ok" (or more precisely, to set up a physical situation
which leads to some provably ok action happening), even though, given any specific such action,
you can (by assumption) prove it's ok.

That means we can't prove it's ok to make the agent (sorry -- I mean to die, but leave around a bunch of
"non-machine stuff" which looks and acts just like the agent), 
even though the agent is only going to take actions we could have proved were ok to do if we'd known what they were.

### a clue

But the humans watching (or imagining)
all this *can* prove that making that agent *could safely be considered ok* (at least when the agent is identical
to the original machine, and under their standard assumption of soundness of the proof-system).
In fact, they can prove that any chain of agent-making actions (of that kind which make identical copies of the machine),
followed by a standard action, is ok. The proof is almost too trivial to write down:

* the world-history consists of a series of original-machine-copy-lifecycles, followed by a non-machine state
(if we admit that the agents are "machines" in spite of how they were made -- we can do that
since it's just a matter of terminology; whatever we're trying to say about the world-history is unaffected by whether
we call some part of it a machine or not);

* by induction, we prove that each machine in turn is a valid copy of the original,
since the prior one was, and made it, and therefore proved making it was ok (by the rule above),
i.e. that the just-made machine was indeed a valid copy,
and that proof was sound so its conclusion was correct;

* the last machine in that series leaves the world in a safe state, by the same proof as before.

This suggests the following partial solution to the Tiling Agents problem:

* allow an agent-making action if it passes a special safety rule,
namely that the agent has to be identical to the original machine.

* also allow a standard action, using the same (version 1) safety rule as before.

We have just proved that the resulting machine is safe, so this is indeed a correct partial solution to Tiling Agents
(and as we saw earlier, it does go beyond what the "version 1" safety rule alone allowed).
But that agent-making safety rule is a bit more limiting than we'd like.

### the solution

Fortunately, there is a way to generalize it without losing the validity of that safety proof:

* allow an agent-making action if it passes a special safety rule,
namely that **the agent has to be a valid implementation of the original machine's abstract specification.**

* also allow a standard action, using the same (version 1) safety rule as before.

Note that the "original machine's abstract specification" is nothing else than the pair of rules we just stated.
(Which means it's a specification that contains a description of itself, but that's not a problem -- the standard
trick for making a program that prints its own source code, or a Gödel sentence, works for this too.)

Everything else about the original machine
(physical structure, nature of representation/implementation of internal states and operations,
purpose, strategy, intelligence, etc)
is **not restricted by the abstract specification**
and is therefore free to vary in the agent relative to the parent machine and/or to the original machine.

But the proof goes through exactly as before, except what we prove by induction is not that each machine in series
is an identical copy of the original, but that it's a valid implementation of the original machine's abstract specification.

### how general is it?

But does that specification permit the full generality we want -- for example, does it cover a "composite machine"
consisting of many original machines working together, with various new physical powers, optimized implementations,
enhanced (but relatively-sound) theorem-proving rules, cooperative strategies, etc?

Yes. The abstract specification only requires that you can prove that the physical structure in question
(the composite machine, aka community of machines) can be correctly described as an implementation of the "abstract program"
described by the two safe-action rules given above, namely, "in whatever manner you want, come up with proposed actions,
prove some of them ok (by one of the two given rules, depending on which type of action you treat it as,
which is an arbitrary choice so you can use whichever one works),
and sometimes do one of those actions". (For convenience of discussion we can then drop the pretense that
each agent-making action turns it into a new machine, and just say there's one long-lived composite machine
performing repeated actions of self-modification.)

(But to spell out the details of that "yes" would require filling out the "theory of programming" mentioned in the summary.
As I said there, I consider it obvious that this can be done, and I would not be surprised if that's well known and/or
also considered obvious by other people. One reason I don't know whether it's considered obvious
is that it's a simple special case of a more general "theory of programming" I have which doesn't seem to be obvious or well-known.
I hope to write more about those things in a future post.)


[update 3, 10/28: It's worth pointing out that
although my formalization separates "safety" and "efficacy"
in a way that seems different from [YH2013],
the difference is mainly terminology.
As far as how those terms are used in the proofs here,
all that matters
is that "safety" is some goal that can be completely formalized,
and which can be attained by some strategy which can be formally proven to work,
whereas "efficacy" can be anything else that might affect the outcome,
like a lower-priority goal addressed by informal or unproven means.
So anything that *can* be fully addressed formally
can be included in the "safety" part of my model.
(And as I've shown, using agents needn't affect
what proof system can be used for addressing it.)

Also [including observations from a discussion
with Tsvi Benson-Tilsen, Andrew Critch, and other members of their seminar on
[Provability, Decision Theory and Artificial Intelligence](http://acritch.com/pdtai/)
(mentioned to credit them, not to imply their endorsement)],
the "abstract specification" can really be any predicate about physical structure of the "composite machine"
(all individual machines and some parts of their surroundings)
which gives the desired effect in the proofs here.
At the most general level, we're just dividing the world-history
(in whatever way is convenient, since it's not a physical distinction)
into "controlled stuff" we make a "design-oriented" assertion about (in as abstract or general a way as we'd like),
and "other stuff" we make a "goal-oriented" assertion about,
in a way that lets us prove
(by means of the "design-oriented" assertion, which should provably
correspond to some high-level designer-like understanding of how the "controlled stuff" behaves)
that the controlled stuff only takes actions which maintain the truth of both assertions.
(To make explicit how time enters into that, both assertions have a time parameter
and apply to the complete world history from the start time to the given time,
with the given time being extended forward by induction.)]


### comparison to related "failed/missing solutions"

[YH2013] discusses some solution attempts which are similar to this one in some ways
(see its section 5, especially footnote 40 and the discussion which references it).
The crucial difference seems to be that they discount
the possibility of a "theory of programming" of the kind I discuss above.
They suggest that any attempt to formalize "system S acts like abstract program G"
will necessarily either be too tight (applying only to physically identical copies of S)
or too loose (applying to systems similar in some parts to S,
but arbitrarily different in others, resulting in arbitrary behavior).
Therefore they conclude that a useful action criterion can only consider the results of S's behavior,
not S's internal structure.

The key element in making a "theory of programming" of the needed kind
is for it to analyze S's overall internal (but abstractly understood and described) behavior
(which is constrained by its structure, but could nonetheless be produced by many different structures).
This does look at S's internal physical structure, not just at its externally visible behavior
(let alone at just the results of that behavior);
but it only requires the structure to have a desired abstract form
(in the same way a "designer of S, with the goal of designing an implementation of G" ought to do),
not any particular physical nature.


#### references

[YH2013] "Tiling Agents for Self-Modifying AI, and the Löbian Obstacle (Early Draft)", Yudkowsky & Herreshoff, 2013,
https://intelligence.org/files/TilingAgentsDraft.pdf


#### comments

The best place for comments, for now, is in email. (The Muut comments below only work for some people.)

(Although this post will be present on my blog, it won't appear for now in the listing of posts on the home page,
since a bunch of people are waiting for my "post #2" on a completely different topic and I don't want to confuse them.)

