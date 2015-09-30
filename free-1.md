# Free project 1

## The user and a language
This section describes who the project would serve and why a language might be a
good way to meet their needs.


### What's the need?
_What need is met by your idea? Who are you helping? What is that person's
experience like now? What would their experience be like if you could help 
them?_

Often, when trying to run programs from the POSIX shell `sh` or related programs
(`bash`, `zsh`, etc.), one needs to chain multiple programs into a pipeline.
Using the command `tee` and some generalized redirection provided by `bash` and
`zsh`, it is possible to make trees of commands instead of simple pipes, but
this can be difficult and doesn’t generalize much further. Ideally, commands
should be easily arrangeable into DAGs, or even non-acyclic directed graphs.

### Why a language?
_Why is a DSL appropriate for your user(s)? How does it address the need?_

This idea is based off of existing shell scripting, and should model it as much
as possible. Commands are often designed to be launched from a text shell, and
making “shell scripts” out of these commands would probably be useful as well.

### Why you?
_What excites you about this idea? How did you come up with it?_

I have had problems with shell scripts not being general DAGs in the past, and I
would like to be able to fix those. I also use the shell a lot, so would be able
to tell when a potential added feature would hurt usability by getting in the
way of other functionality.

### Domain
_Describe the project's domain in five words._

Generalized I/O pipelines in shell

### Interface (syntax)
_How might the user interact with the language? What does programming look 
like? Why is this the right way to interact with the problem domain?_ 

Maintaining the shell-like syntax is probably best, if possible. Simple things,
like running a pipeline without without fancy pipelines, should work exactly as
in the regular shell. Fancier stuff should be easy to define and use, to give a
practical advantage over existing named pipes in a `tmpfs` setting.

Probably the easiest way to handle it would be like with shell variables, except
allow `$X` to be set to a pipe or socket instead of text or a number. Then it
could be automaically handled with both redirection (e.g.,
`echo Hello, world > $X`) and passing as a parameter (e.g., `cat $X`)

### Operation (semantics)
_What might happen when a program runs? How does a program interact with the
user? What kinds of errors might occur, and how might they be communicated to
the user?_

Primarily, this would work like existing shell scripts: each line executes in
order (except for within specifc control flow structures), and either it forks
and execs one or more programs, or it controls internal information (like shell
variables); it could possibly do both (like the traditional shell script line
`export NOW=$(date)`).

The difference is that in this language, the lines which run several programs
could hook them up in more expressive ways than traditional shell scripts can.

### Expressiveness
_What should be easy to do in this language? What should be possible, but
difficult? What should be impossible or very difficult?_

In this language, it should be easy to make arbitrary DAGs connecting commands’
input and output together. It should be possible to make cyclic graphs, but this
should probably be more difficult because it’s not usually the desired outcome.
Pretty much any traditional general-purpose function should be very difficult,
because those would be handled by calling the right programs instead of doing it
in the shell.

### Related work
_Are there any other DSLs in this domain? If not, describe how you know there
aren't and conjecture why not. If so, describe them and provide links. How well 
do they address the need? Are there any particularly admirable qualities of the
language? Are there parts of the language you think could be improved?_

There are *plenty* of other POSIX shells. The original Bourne shell, the
[Korn Shell](http://www.kornshell.org/),
the [Bourne Again SHell](https://www.gnu.org/software/bash/),
the [Z Shell](http://zsh.sourceforge.net/), and even the C shell and
[`tcsh`](http://www.tcsh.org/Welcome) all handle the domain of launching other
processes and redirecting their outputs to some extent, but they don’t handle
the complex graphs intended in this language.

Meanwhile, there are also languages for producing graphs, most notably
[DOT](http://www.graphviz.org/content/dot-language). However, none of these are
designed to produce graphs that are executable in quite the same manner.

A final area worth mentioning are build tools like `make` (most notably the
[GNU version](https://www.gnu.org/software/make/)),
[SCons](http://www.scons.org/), [tup](http://gittup.org/tup/), and
[SBT](http://www.scala-sbt.org/). These all create DAGs of commands to run, but
they are designed for making reproducible scripts instead of for REPL-like
interpretation. They also usually require you to keep intermediate products on
disc, instead of using FIFOs in RAM.

This project mainly takes its inspiration from the first set, the shells.

## The Project
This section examines whether the idea makes for a good CS 111 project.


### Suitability
_If someone were to work on this project, what percentage of their time would be
spent directly engaging in the **language** aspects of this project (e.g.,
making language design decisions), as opposed to "systems" aspects of the
project (e.g., implementing a complicated semantics that doesn't require a lot
of language design)?_

Probably the majority of the time would be language-related. The implementation
is relatively simple (assuming it isn’t cross-platform); the API to create pipes
already exists in the `pipe(2)` system call. Other potential features get more
complicated, but the system calls are still all relatively simple and the main
part of the work would be the language.

One potential pitfall would be that it might take a while to reimplement
existing shell syntax. A minimal Bourne-compatible shell would probably be
almost trivial to implement, but adding the features that users of `bash` and
`zsh` are accustomed to would take forever before further language design even
became an issue. If this can be implemented as an internal DSL within shell
scripting, or as an extension to `zsh` (which I know to be extensible, although
I know nothing about the extension mechanisms), then this pitfall is avoided.

### Scope
_How big an idea is this? How ambitious is this project?_

That’s hard to tell. A very simple, usable prototype could probably be completed
in a few hours, which is obviously too small. There are a lot of features it
would be nice to include (expanding to sockets instead of just pipes, adding
I/O via tmpfs files instead of pipes, adding wrappers around commands to make
them work more uniformly or programmatically, etc.), but it’s difficult to know
whether any of those features are integrable into the language in a reasonable
way.

I expect that, if a language feature could be developed for wrappers (similar
to the existing means of defining functions in Bourne-like shells), then the
project would have a potentially large scope. I’m not sure about how feasible it
would be to integrate such a method with existing `sh`-like syntax, though.

### Benefits and drawbacks
_Why might this be a good idea for a project? Why might this not be a good idea 
project?_

This could be a good idea for a project because, assuming the issues in the
previous two sections were resolved, there is a lot of freedom for language
design. There would relatively little difficulty in implementing the language,
and the language could be built in an agile way so that there was a working
deliverable product at the end of the semester regardless of potential poor
estimation of scope.

If the problems of the last two sections (not reimplementing a full-featured
shell; allowing command wrappers for uniformity) were not solved or otherwise
avoided, then the project would be either far too large in scope or far too
small in scope.
