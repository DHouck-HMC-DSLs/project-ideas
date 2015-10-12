# Anti project


## The user and a language
This section describes who the project would serve and why a language might be a
good way to meet their needs.


### What's the need?
_What need is met by your idea? Who are you helping? What is that person's
experience like now? What would their experience be like if you could help 
them?_

Passwords are universal, but they are not actually very good for security. In
theory, you can’t log in unless you know the password, meaning you’re either the
account owner or they explicitly let you log in, and website password rules
ensure that users pick good passwords. In practice, users are rarely able to
remember a lot of different passwords, so they reuse the same one, and they care
enough about memorable passwords that they circumvent rules trying to enforce
good passwords.

This can be mitigated by generating passwords randomly and storing them in a
secure password manager (e.g., [LastPass](https://lastpass.com/)), or by
generating them pseudorandomly and using website details and a master password
as the seeds of the PRNG (e.g., [PasswordMaker](http://passwordmaker.org/)). The
second is preferable in many cases because it only requires the user to remember
the one master password and not worry about losing anything, and it does not
require the user to trust the website keeping track of their passwords, but
often the generated passwords fail to match websites’ actual password rules and
it is difficult to generate (and remember the settings for) a password which
does.

### Why a language?
_Why is a DSL appropriate for your user(s)? How does it address the need?_

A DSL is not an appropriate solution to the problem. It requires the user to
write down the rules for allowed passwords for a website before logging in, or
at least before signing up for an account. This is an annoying step which is
often impossible because many websites don’t correctly advertise their password
rules. If this were required, it would be difficult to use the program and users
would go back to insecure password solutions.

### Why you?
_What excites you about this idea? How did you come up with it?_

I always try to be secure online, and I care more about information security
than many other people do. I have thought about trying to implement a solution
to this problem before, but never done it given other projects getting in the
way.

### Domain
_Describe the project's domain in five words._

Repeatable password generation given schemas

### Interface (syntax)
_How might the user interact with the language? What does programming look 
like? Why is this the right way to interact with the problem domain?_ 

The user would write down the password rules as specificed by the website in the
DSL, for example:

```
8 to 16 total
0 symbols
1+ capital
1+ lowercase
3+ numbers
3  MaxConsecutiveRepeat
3  MaxSequence
```

### Operation (semantics)
_What might happen when a program runs? How does a program interact with the
user? What kinds of errors might occur, and how might they be communicated to
the user?_

The user would run the program, be asked for further details for generating the
password (eg., the URL, the username, and a master password), and would then
have the generated password in their clipboard for pasting into the website.

If the rules are contradictory, then the program should fail to run.

### Expressiveness
_What should be easy to do in this language? What should be possible, but
difficult? What should be impossible or very difficult?_

The program should ideally allow all common sorts of password rules: no common
dictionary words, other forbidden sequences, maximum and minimum numbers of
characters of any given type, a specific list of allowed or forbidden symbols,
etc. It should be difficult but possible to express most context-free password
requirements, and impossible to specify requiremens where checks might not
terminate.

I don’t remember if the problem of checking whether a context-free grammar is
empty is solvable, so the above paragraph might itself be contradictory.

### Related work
_Are there any other DSLs in this domain? If not, describe how you know there
aren't and conjecture why not. If so, describe them and provide links. How well 
do they address the need? Are there any particularly admirable qualities of the
language? Are there parts of the language you think could be improved?_

There are DSLs for specifying other grammars, but I doubt there are any for
specifying password rules in particular, probably because that would be a bad
idea.

## The Project
This section examines whether the idea makes for a good CS 111 project.


### Suitability
_If someone were to work on this project, what percentage of their time would be
spent directly engaging in the **language** aspects of this project (e.g.,
making language design decisions), as opposed to "systems" aspects of the
project (e.g., implementing a complicated semantics that doesn't require a lot
of language design)?_

Probably a majority of the project would be finding a language that can express
all or most common password rules, followed by writing a generator which can
create passwords in that language.

### Scope
_How big an idea is this? How ambitious is this project?_

The password part of the project for coding a PRNG from user input is already
solved in PasswordMaker. The part about writing the language might or might not
take a while, depending on how difficult it is to find a way to easily express
password rules in a machine-readable way. The part about generating strings to
fit the described language of valid passwords is difficult to estimate, but I
expect would be of some medium difficulty. Overall, the project would be
somewhat ambitious.

### Benefits and drawbacks
_Why might this be a good idea for a project? Why might this not be a good idea 
project?_

This could be a good idea for a project because it involves designing a DSL for
a weird domain, but it would be bad in most other ways: a DSL is not the right
way to solve this problem, certainly not by itself, and the main body of work
would not be on implementing the language but the backing library.
