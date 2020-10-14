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
