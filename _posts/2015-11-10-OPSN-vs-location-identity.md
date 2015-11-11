---
layout: post
title:  "comparison to location-based object identity"
listed: false
date:   2015-11-10 17:32:36
author: "Bruce Smith"
part_of_EMSC: true
---


A URL ("uniform resource locator") is a useful standard way of finding an object using a symbolic location.

(Although it doesn't directly specify a physical location,
it does direct users to interact with "whatever object is found by using it at a given time,
finding that object using an external address-resolution system which interprets the symbolic location".
(The found object is typically a web server or something hosted by one.)
So it is more correctly described as a reference to "whatever's found at a symbolic location, right now"
than to "whatever has a certain identity, whereever and whenever it might be found".
Thus we describe it as using "location-based object identity"
rather than "hash-based immutable data identity".)

The objects found using a URL can be live, stateful objects to interact with,
or pure data (which might be dynamic or static).
The use of URLs to refer to static data is a useful special case.
But as a system of references to immutable data, which need to be reliable and long-lasting
(for as long as anyone is willing to publicly store the data),
the URL has many problems:

* there is no standard way to verify the correct data was found.

* there is no guarantee that the same data can be found later, either in the same place or anywhere else.
That is not just a theoretical issue -- trying to follow several-year-old conversations on the web leads
to "broken links" as often as not.

* even when someone is willing to archive data in other locations,
in case it later can't be found at the original location,
this is difficult to do effectively:

  * there is no standard automatic way to determine if this is legally permitted for that data
  (though there are approximations to this);

  * there is no standard way for someone to announce, or even record, that they've done this,
  in a form that could efficiently and automatically help people searching for that data in the future;

  * if that data is referred to by other data (for example if it's a node in a distributed graph),
  or in some cases (when location-relative reference formats are used) even when it itself *refers* to other data,
  there is no standard way for those connections
  to be maintained, when the data is moved or copied.

* the standard systems for mapping URLs to physical locations (DNS),
and for secure communication with the resulting locations (HTTPS), are centrally administered
(for registration of their "official definitions", i.e. domain names and certificates)
and not fully secure.
Also, domain ownership (and thus ownership of all URLs under the domain) can be sold or lost.
(In the long run, it's even possible that DNS could be "forked" for political reasons,
so certain domain names would resolve differently in different countries.)
So even under ideal circumstances,
a data reference whose meaning is just "whatever data is found at a certain URL"
can't be considered permanent or reliable in the long run.

The inventor of the URL, Tim Berners-Lee, recognized many of these problems and took them seriously.
He intended the URL to be one of several related inventions,
with others (including other kinds of "uniform resource identifiers" or URIs, some of which would not be location-based)
being developed to solve some or all of the problems mentioned above.
Some of that work is still ongoing, but it hasn't been as fully developed (let alone as widely used) as the URL.
(In particular, the only widely-used hash-based URIs are supported in specialized p2p systems
(which have different design goals and infrastructure needs than OPSN), but not in standard browsers.)

Several other proposals have been made
to solve related problems (and some of them have come into limited use),
such as [IPFS](http://ipfs.io/);
so have some proposals to solve analogous
problems about references to live (interactive, stateful) objects.
(Making live objects and their references reliable and permanent is probably a harder problem,
but mainly it's just a different problem.
A good solution to it *might* specialize to solve this problem well too,
but since this one is simpler, it seems better to consider it more primitive and solve it separately.)

Unfortunately none of these systems has become widespread enough to fix the "broken link" problem in practice,
let alone the other listed problems.
Also, to my knowledge, none of them has all the properties that this document argues
are needed for OPSN and that the present proposal has:

* references to data are universal (work without change from any time and context),
and are reliable as long as someone publicly stores the data,
provided certain community practices are followed
(to protect against obsolescense of hash functions,
and publish location hints when data is rehosted);

* anyone willing to host data (or able to formally describe an existing compatible host)
can create a universal reference to it;

* anyone can copy or move data without loss of its connections to other objects or data;

* in particular, anyone can make a fully-functional backup of whatever data they consider most important.

As a reminder (since details are spread throughout many of the posts which make up this document),
the main features of the present proposal which provide the above properties are:

* references based on hashes of immutable data,
treating mutable objects as derived from immutable primitive data items;

* treating data copying and rehosting as routine, and explicitly permitted
(so as to tolerate any level of data loss short of complete loss of all public copies);

* using OPSN collaboration mechanisms (both automatic and manual)
for community maintenance of a system of references, including hash function integrity.

As always, the design choices that enable those advantages come with tradeoffs:

* when a location-based reference *does* work and is trusted, using it is simpler and more efficient.

* when data needs to be updated (and its users want the updated data rather than the original),
this is straightforward with a location-based reference,
but requires a more complex higher-level construction with a content-based reference
(and an update can only be verified as "legitimate" by being signed or by community discussion).

  * but the needed higher-level constructions for updates can be automated,
  so using them can be just as simple in the end;
what is gained
(the updates can't masquerade as the original data -- anyone can see their source and that they don't equal the original --
and both updated and original data can be reliably and permanently available)
more than compensates for what is lost
(a simpler way to get the latest data, but which only works under fragile conditions).

* sometimes there are legitimate reasons to remove or prohibit specific data from the system, or restrict access to it;
if every user can store and serve copies of data (and often does), enforcing related rules might be harder.

  * but that is a higher-level issue, not a technical one,
  and also applies to the internet as a whole rather than just to any specific protocol;
it should be addressed at the social and legal levels,
rather than by not having a fundamental data-reference-and-distribution network protocol
with the minimal necessary properties for reliability, like safeguarding persistence of high-value data.

  * also, this system doesn't really make it technically harder to address that issue --
  for example, it permits a formalization of removal requests,
  and of reasons for them and (potentially automated) policies for responding to them;
  thus it is neutral to the debate about them, but makes any particular policy simpler to implement.

