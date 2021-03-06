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

### Steady State

Every single time a human touches a server is an opportunity for unforced
errors

the system should be able to run at least one release cycle without human intervention.

The Steady State pattern says that for every mechanism that accumulates a
resource, some other mechanism must recycle that resource.

### Data Purging

Data purging is nasty, detail-oriented work. Referential integrity constraints
in a relational database are half the battle. The other half of the battle is
ensuring that applications still work once the data is gone.

### Log Files

Log file rotation requires just a few minutes of configuration.

### In-Memory Caching

Improper use of caching is the major cause of memory leaks

### Fail Fast

If the system can determine in advance that it will fail at an operation, it’s
always better to fail fast.

If any of the
resources are not available, the service can fail immediately

### Let it crash

Sometimes the best thing you can do to create system-level stability is to
abandon component-level stability.

There’s just no way to test everything or predict all the ways a system can
break. We must assume that errors will happen.

The cleanest state your program can ever have is right after startup. The “let
it crash” approach says that error recovery is difficult and unreliable, so our
goal should be to get back to that clean startup as rapidly as possible.

#### Limited granularity

We want to crash a component
in isolation. The rest of the system must protect itself from a cascading failure.

In Erlang or Elixir, the natural boundary is the actor. 

Other languages have actor libraries, such as Akka for Java and
Scala. 1 These overlay the actor model on a runtime that has no idea what an
actor is.

In a microservices architecture, a whole instance of the service might be the
right granularity.

#### Fast replacement

With in-process components like actors, the restart time is measured in
microseconds. Callers are unlikely to really notice that kind of disruption.

Service instances are trickier. It depends on how much of the “stack” has to
be started up

Startup time is measured in minutes.
“Let it crash” is not the right strategy.

#### Supervision

Actor systems use a hierarchical tree of supervisors to manage the restarts.

Supervisors need to keep close track of how often they restart child processes.
It may be necessary for the supervisor to crash itself if child restarts happen
too densely.

In a virtualized environment with autoscaling, the
autoscaler decides whether and where to launch a replacement. They will
always restart the crashed instance, even if it is just going to crash again
immediately.

#### Reintegration

The final element of a “let it crash” strategy is reintegration. the instance should
be reintegrated when health checks from the load balancer begin to pass.

### Handshaking

Handshaking refers to signaling between devices that regulate communication
between them.

Handshaking is
ubiquitous in low-level communications protocols but is almost nonexistent
at the application level.

HTTP provides a response code of “503 Service Unavailable,”. 

the
server should have a way to reject incoming work.

When there are several services, each can provide a “health check” query for
use by load balancers. The load balancer would then check the health of the
server before directing a request to that instance. This provides good hand-
shaking at a relatively small expense to the service.

Circuit Breaker is a stopgap you can use when calling services that cannot
handshake.

### Test Harnesses

Integration testing presents problems of its own, however. What version should
we test against?

it’s vital to test the local system’s
behavior when the remote system goes wonky.

there will be behaviors that
integration testing does not verify.

you can create test harnesses to emulate the remote system on
the other end of each integration point.

A test harness runs as a
separate server, so it’s not obliged to conform to any interface. It can provoke network
errors, protocol errors, or application-level errors.

> I think Wiremock could be a test harness?

### Decoupling Middleware

Often described as “plumbing”

Any kind of synchronous call-and-response or request/reply method forces the
calling system to stop what it’s doing and wait.

Less tightly coupled forms of middleware allow the calling and receiving sys-
tems to process messages in different places and at different times. any queue-based or publish/subscribe messaging
systems fall into this category

Message-oriented middleware decouples the endpoints in both space and
time.

Designing asynchronous processes is inherently harder.

### Shed load

No matter how strong your load balancers or how
fast you can scale, the world can always make more load than you can handle.

Services should model TCP’s approach. When load gets too high, start to
refuse new requests for work. This is related to Fail Fast.

When requests take longer than the SLA,
it’s time to shed some load.

### Create back pressure

The mechanisms
change but the idea is still to slow the producer down until the consumer
can catch up.

The Back Pressure pattern works best with
asynchronous calls and programming.

back pressure works best within a
system boundary.

When Back Pressure kicks in, monitoring needs to know about it.

### Governor

We should use automation for things
humans are bad at: repetitive tasks and fast response. We should use humans
for what automation is bad at: perceiving the whole situation at a higher level.

The whole point of a governor is to slow things down enough for humans to
get involved.

## Case Study: Phenomenal Cosmic Powers, Itty-Bitty Living Space

a Calabrian doctor named Aloysius Lilius invented a new
calendar to fix a bug in the widely used Julian calendar. became known
as the Gregorian calendar rather than the Lilian calendar.

Each industry has its own internal almanac.

For retailers, the year begins and ends with the euphemistically named “holiday
season.

Shopper panic sets in,
resulting in a collective phenomenon known as Black Friday. Retailers
encourage and reinforce this by changing their assortment, increasing stocks
in stores, and advertising wondrous things.

This is the real load test, the only one that matters.

### Baby's first christmas

My client had launched a new online store in the summer.

for all the problems we experienced following the launch, we approached
the holiday season with cautious optimism.

Our optimism was rooted in several factors. First, we had nearly doubled the
number of servers in production. Second, we had hard data showing that the
site was stable at current loads.

we had more visibility and control over the online store internals than any other system on which
I’ve worked.

Our local team was far too small to be on-site twenty-four hours a
day all the time, but we worked out a way to do it for the limited span of the
Thanksgiving weekend.


### Taking the pulse

During the run-up to the launch, I was part of load testing this new site.

To get more information out of the load test, I had started off using the
application server’s HTML administration GUI to check vitals like latency,
free heap memory, active request-handling threads, and active sessions.

> Monitoring

Monitoring technology provides a
great safety net, pinpointing problems when they occur, but nothing beats
the pattern-matching power of the human brain.

### Thanksgiving day

The session count in the
early morning already rivaled peak time of the busiest day in a normal week.

By noon, customers had placed as many orders as in a typical week.

Page
latency, our summary indicator of response time and overall site performance,
was clearly stressed but still nominal.

By midnight, we had taken as many orders
as in the entire month of October—and the site held up. It passed the first
killer load test.

### Black friday

Orders were trending even higher than the day before.
Session counts were up, but page latency was still down around 250 millisec-
onds, right where we knew it should be.

Of course, I wouldn’t be telling this story if things didn’t go horribly wrong.

SiteScope
simulates real customers, as shown in the
figure. When SiteScope goes red, we know
that customers aren’t able to shop and we’re
losing revenue.

“All DRPs red” meant the site was down, losing orders at a
rate of about a million dollars an hour. “Rolling restart” meant they were
shutting down and restarting the application servers as fast as possible.

### Vital signs

- Session counts were very high, higher than the day before.
- Application server page latency (response time) was high.
- Web, application, and database CPU usage were low—really low.
- Request-handling threads were almost all busy.

Response time is always a lagging indicator. You can only
measure the response time on requests that are done.

Requests that didn’t complete never got averaged in.

### Diagnostic tests

The waiting threads were all blocked on a resource pool,
one that had no timeout. If the back end stopped responding, then the threads
making the calls would never return, and the ones that were blocked would
never get their chance to make their calls.

Attention swung to the order management system. Thread dumps on that
system revealed that some of its 450 threads were occupied making calls to
an external integration point

### Call in a specialist

He explained that of the four servers that
normally handle scheduling, two were down for maintenance over the holiday
weekend and one of the others was malfunctioning for reasons unknown.

That left us with a huge imbalance in the sizes of the systems, as shown in
the following figure. The sole scheduling server that remained could handle
up to twenty-five concurrent requests before it started to slow down and hang.

our business sponsor gravely informed us that marketing
had prepared a new insert that hit newspapers Friday morning. The ad offered
free home delivery for all online orders placed before Monday.

### Compare treatment options

It quickly became clear that the only answer was to stop
making so many requests to check schedule availability.

We saw a glimmer of hope when we looked at the code for the store. it had a separate connection pool just for scheduling
requests. it had a component just for those connec-
tions, we could use that component as our throttle.

### Does the Condition Respond to Treatment?

I used the script to set max for that resource pool (on just
one DRP) to zero, and I set checkoutBlockTime to zero. 

I used another script, one that could invoke methods on the component, to
call its stopService() and startService() methods. Voilà! That DRP started handling
requests again!

The ability to restart components, instead of entire servers, is a key concept
of recovery-oriented computing.

Recovery-Oriented computing

- Failures are inevitable, in both hardware and software.
- A priori prediction of all failure modes is not possible.
- Human action is a major source of system failures.

Their investigations aim to improve
survivability in the face of failures

Dynamically reconfiguring and restarting just the connection pool took less
than five minutes

### Winding down

I wrote a new script that would do all the actions needed to reset that connec-
tion pool’s maximum.

an engineer in the operations center
or in the “command post” (that is, the conference room) at the client’s site
could reset the maximum connections to whatever it needed to be.


