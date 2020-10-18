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


