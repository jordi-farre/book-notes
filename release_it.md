# Release It by Michael T.Nygard

## Chapter 1

### Living in Production

Software design as taught today is terribly incomplete. It only talks about what systems should do. It doesn't address the converse, what systems should not do.

You will need to accept the fact that despite your best laid plans, bad things will still happen.

### Aiming for the Right Target

It aims to survive the artificial realm of QA, not the real world of production.

### The scope of the challenge

The increasing scope of this challenge - to build software fast that's cheap to build, good for users, and cheap to operate - demands continually improving architecture and design techniques. 

### A million dollars here, a million dollars there

Avoiding a one-time developmental cost and instead incurring a recurring operational cost makes no sense.

Design and architectural decisions are also financial decisions.

### Use the force

The earliest decisions you make can be the hardest ones to reverse later.

Production is the only place to learn how the software will respond to real-world stimuli. 

### Pragmatic architecture

An architect doesn't bother to listen to the coders on the team doesn't bother listening to the users either. 

## Chapter 2

### The Outage

In any incident, my first priority is always to restore service. Restoring service takes precedence over investigation.

> Restart as solution? Why? I think because it's something easy to try and sometimes solves the problem. I understand that recovering the service is the first priority, but sometimes restarting will not solve the problem and maybe you will lose some valuable time. 

### Postmortem

> Good thing to do a postmortem, sadly sometimes this practices are not done.

“post hoc, ergo propter hoc”—Latin for “you touched it last”—turns out to be
a good starting point most of the time. It’s not always right, but it certainly
provides a place to begin looking

managing perception after a major incident can be as important as managing the incident itself.

> a postmortem should be done even if management doesn't ask for it.

### Hunting for clues

Armed with a thread dump, the application is an open book, if you know how to read it. You can deduce a great deal about applications for which you’ve never seen the source code

CORBA (dead as disco) xD

### The smoking gun

The key lesson to be drawn here, though, is that the JDBC specification allows java.sql.Statement.close() to throw a SQLException , so your code has to handle it.

> Which is the correct way to handle this? retry the close again? some alert?

Ultimately, it’s just fantasy to expect every single bug like this one to be driven out. Bugs will happen. They cannot be eliminated, so they must be survived instead.

Inside every enterprise today is a mesh of interconnected, interdependent systems. They cannot—must not—allow bugs to cause a chain of failures

## Chapter 3

### Stabilize your system

Things happen in the real world that just don't happen in the lab -usually bad things-

When building the architecture,
design, and even low-level implementation of a system, many decision points
have high leverage over the system’s ultimate stability.

The amazing thing is that the highly stable design usually costs
the same to implement as the unstable one.

### Defining stability

A transaction is an
abstract unit of work processed by the system.

The word system means the complete, interdependent set of hardware,
applications, and services required to process transactions for users

A robust system keeps processing transactions.

An impulse
is a rapid shock to the system. An impulse to the system is when something
whacks it with a hammer. In contrast, stress to the system is a force applied
to the system over an extended period.

### Extending your life span

The major dangers to your system’s longevity are memory leaks and data
growth.

Following Murphy’s Law,
whatever you do not test against will happen.

The only way you can catch them
before they bite you in production is to run your own longevity tests.

If all else fails, production becomes your longevity testing environment by
default. You’ll definitely find the bugs there

> Maybe if you have good monitoring and alerting systems, you can detect and fix these bugs very quickly and you can avoid to have this long running tests in development.

### Failure modes

Once you accept that failures will happen, you have the ability to design
your system’s reaction to specific failures. You can create safe failure modes that contain the damage and protect the rest of the system.

### Stopping crack propagation

The pool
could have been configured to create more connections if it was exhausted.
It also could have been configured to block callers for a limited time, instead
of blocking forever when all connections were checked out.

The client could have been written to set a timeout on the RMI sockets.

the CF servers themselves could have been partitioned
into more than one service group.

The more tightly coupled the architecture, the greater the
chance this coding error can propagate.

### Chain of failure

If you tried to estimate
the probability of that exact chain of events occurring, it would look incredibly improbable.

- Fault A condition that creates an incorrect internal state in your software.

- Error Visibly incorrect behavior.

- Failure An unresponsive system.

One way to prepare for every possible failure is to look at every external call,
every I/O, every use of resources, and every expected outcome and ask, “What
are all the ways this can go wrong?”

Faults will happen; they can never
be completely prevented. And we must keep faults from becoming errors.

## Chapter 4

### Stability Antipatterns

As we integrate the world, tightly coupled systems are the rule rather than the exception.

High interactive complexity arises when systems have enough moving parts
and hidden, internal dependencies that most operators’ mental models are
either incomplete or just plain wrong.

the operator’s instinctive actions will have results ranging from
ineffective to actively harmful.

Tight coupling allows cracks in one part of the system to propagate themselves
—or multiply themselves—across layer or system boundaries.

### Integration Points

A butterfly has
a central system with a lot of feeds and connections fanning into it on one
side and a large fan out on the other side

Some people would call this a monolith, but that has negative connotations.
It might be a nicely factored system that just has a lot of responsibility.

The other style is the spiderweb, with many boxes and dependencies.

the more we move toward a large number
of smaller services, the more we integrate with SaaS providers, and the more
we go API first, the worse this is going to get.

> Microservices architecture is antipattern?

### Socket based protocols

is that it can take a long time to discover
that you can’t connect.

a slow response is a lot worse than no response.

### The 5AM problem

Whether for a problem diagnosis or performance tuning, packet capture tools
are the only way to understand what’s really happening on the network.

> I used this once, to diagnose a problem with SSL connections done from an external server to our API. It was really useful, but a desperate measure in my case.

not every problem can be solved at the level of
abstraction where it manifests

### HTTP Protocols

all HTTP-based protocols use sockets, so they are vulnerable to
all of the problems described previously, but adds its own set of issues.

The provider may accept the TCP connection but never respond to the
HTTP request.

The provider may accept the connection but not read the request.

The provider may send back a response status the caller doesn’t know
how to handle.

The provider may send back a response with a content type the caller
doesn’t expect or know how to handle,

The provider may claim to be sending JSON but actually sending plain
text.

### Vendor API libraries

It would be nice to think that enterprise software vendors must have hardened
their software against bugs, but it's rarely true for their client libraries.

The worst part about these libraries is that you have so little control over
them.

> Some payment providers offered us client libraries, but we preferred to use directly their REST API, to have more control.

> We don't know the insides of the library and in case of error will be difficult to know what's happening.

### Chain Reactions

A chain reaction occurs when an application has some defect—usually a
resource leak or a load-related crash.

### Cascading failures

A cascading failure occurs when a crack in one layer
triggers a crack in a calling layer.

Every dependency is a chance for a failure to cascade.

Crucial services with a high fan-in—meaning ones with many callers—spread
their problems widely, so they are worth extra scrutiny.

### Users

Users are a terrible thing

> lol XD

### Traffic

How does your system react to excessive demand?

then autoscaling is your friend. But beware!
It’s not hard to run up a huge bill by autoscaling buggy applications.

> How we know when to scale up? By CPU metrics?

### Heap memory

Your best bet is to keep as little in the in-memory session as possible.

Weak references are a useful way to respond to changing memory conditions,
but they do add complexity. When you can, it’s best to just keep things out
of the session.

### Off-Heap memory, Off-Host Memory

Memcached or Redis, for example.

### Expensive to Serve

There is no effective defense against expensive users. They are not a direct
stability risk, but the increased stress they produce increases the likelihood
of triggering cracks elsewhere in the system.

The best thing you can do about expensive users is test aggressively. Load tests with more proportion than expected.

### Unwanted users

An entire parasitic industry exists by consuming resources from other com-
panies’ websites. Collectively known as competitive intelligence companies,

Once you identify a screen scraper, block it from your
network.

Write some terms of use for your site that say
users can view content only for personal or noncommercial purposes. Then,
when the screen scrapers start hitting your site, sic the lawyers on them.

### Malicious users

Truly talented crackers who can analyze your defenses, develop a customized
attack, and infiltrate your systems without being spotted are blessedly rare.
This is the so-called “advanced persistent threat.”

### Blocked threads

Here’s the catch
about interpreted languages, though. The interpreter can be running, and
the application can still be totally deadlocked, doing nothing useful.

Multithreading is complex

> Some languages provide an Actor model as a promise of a better multithread management I think.

A mock client somewhere (not in the same data center) can run synthetic
transactions on a regular basis. That client experiences the same view of the
system that real users experience.

> Maybe a little portion of e2eTests running on an schedule in production?

If you find yourself synchronizing methods on your domain objects, you
should probably rethink the design

“Command Query Responsibility Separation,” and
it nicely avoids a large number of concurrency issues.

### Spot the blocking

When misused, however, caching can create new problems.

### Libraries

Libraries are notorious sources of blocking threads, whether they are open-
source packages or vendor code

> In an opensource library you can propose improvements.

### Self-Denial attacks

Good marketing can kill you at any time

### Avoiding Self-Denial

You can avoid machine-induced self-denial by building a “shared-nothing”
architecture.

> I think this is difficult, because applications so many times have something shared between differents instances, like databases.

“pre-autoscale” by upping the configuration before the marketing
event goes out.

> In my previous work they did this before a black friday.

> I think autoscaling should be fast enough if your applications have a low startup time.

### Scaling effects

Because the development and test
environments rarely replicate production sizing, it can be hard to see where
scaling effects will bite you.

### Point-to-point communications

Publish/subscribe messaging is better still, since a server can pick
up a message even if it wasn’t listening at the precise moment the message
was sent. Of course, publish/subscribe messaging often brings in some
serious infrastructure cost. This is a great time to apply the XP principle that
says, “Do the simplest thing that will work.”

### Shared Resources

When the shared resource gets overloaded, it’ll become a bottleneck limiting
capacity

### Unbalanced capacities

The fact is that the front end
always has the ability to overwhelm the back end, because their capacities
are not balanced.

### Drive Out Through Testing

what can you do if your service serves such unpredictable callers? Be
ready for anything.

> Makes sense to test with a load that maybe you never will have?

### Dogpile

When a bunch of servers impose this transient load all at once, it’s called a
dogpile.

### Slow responses

slow response, on the other hand, ties up resources in
the calling system and the called system.

You should give your system the ability to monitor its own performance

### Unbounded Result Sets

Design with skepticism, and you will achieve resilience

## Chapter 5

### Timeouts

Every application must
grapple with the fundamental nature of networks: networks are fallible

Well-placed timeouts provide fault isolation

Timeouts can also be relevant within a single service. Any resource pool can
be exhausted.

Also beware of language-level synchronization or mutexes. Always use the
form that takes a timeout argument.

Use a generic gateway to provide the template for connection handling,

> Maybe using a framework like Spring provides you this kind of handling.

fast retries are very likely to
fail again.

queuing the work for a slow retry later is a good thing,
making the system more robust

### Circuit Breaker

When the circuit is “open,” calls to the circuit breaker fail immediately,
without any attempt to execute the real operation.

A circuit
breaker may also have a “fallback” strategy.

> I find difficult to have a fallback strategy for all the situations. When you use an external provider, maybe you don't have another alternative to do the fallback.

Leaky Bucket pattern. It’s a simple counter that you can increment every
time you observe a fault. In the background, a thread or timer decrements
the counter periodically

### Bulkheads

Physical
redundancy is the most common form of bulkheads

mission-critical service might be implemented as sev-
eral independent farms of servers,

> AWS zones for example?

Dynamic partitions can be made and
destroyed as traffic patterns change.


