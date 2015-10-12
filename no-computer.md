# No computer project


## The user and a language
This section describes who the project would serve and why a language might be a
good way to meet their needs.


### What's the need?
_What need is met by your idea? Who are you helping? What is that person's
experience like now? What would their experience be like if you could help 
them?_

Electric circuits are often made from the same types of parts with one or two
parameters changed. These parts include resistors, capacitors, inductors,
batteries or other power sources, diodes (including LEDs), lightbulbs, motors,
and many other components. Some of these components have parameters (for
example, resistors can have different resistances, and capacitors can have
different capacitances, and lightbulbs have both a resistance and a curve for
how bright they are at different powers). Without a language for expressing
these parts, it can be difficult to describe what different circuits are and how
they work.

### Why a language?
_Why is a DSL appropriate for your user(s)? How does it address the need?_

A DSL for expressing circuit connections could help people read and understand
different circuits, and could even be used when designing circuits if it were
easy to write on scratch paper.

### Why you?
_What excites you about this idea? How did you come up with it?_

I would be a terrible choice for implementing this DSL; I merely recognize its
existance.

### Domain
_Describe the project's domain in five words._

Describing Electric Circuits from Components

### Interface (syntax)
_How might the user interact with the language? What does programming look 
like? Why is this the right way to interact with the problem domain?_ 

Unlike most languages, this would not be best expressed as a stream of text. The
different electrical components connect in nonlinear ways, and would be better
expressed using a graphlike structure.

There would be different symbols for the different components. Certain parts of
the symbols would be designated for connecting wires, which could be added by
drawing lines between the symbols. Wires could also be connected by simply
intersecting the lines representing those wires and drawing a dot. When an
electrical component would take a parameter, such as the voltage of a battery,
it could be expressed by simply writing the relevant parameter next to the
symbol for that component.

### Operation (semantics)
_What might happen when a program runs? How does a program interact with the
user? What kinds of errors might occur, and how might they be communicated to
the user?_

This is a DSL for expressing data; it would not really be run. The semantics
would be that the symbols represent various components and the lines represent
wires between those components. One way to execute the program would be to take
the source and build a circuit that matches that diagram.


### Expressiveness
_What should be easy to do in this language? What should be possible, but
difficult? What should be impossible or very difficult?_

It should be easy to represent many sorts of electric circuits. Different
“libraries” could be used to represent different sorts of components, but
writing those libraries would be outside the scope of the language.

The difficulty should generally scale linearly with the number of components in
a circuit, and should scale in a complex way with how those circuits are
connected. It should be difficult but not impossible to represent a circuit
which does not correspond to a planar graph.

It should be impossible to represent most things that are not electric circuits.

### Related work
_Are there any other DSLs in this domain? If not, describe how you know there
aren't and conjecture why not. If so, describe them and provide links. How well 
do they address the need? Are there any particularly admirable qualities of the
language? Are there parts of the language you think could be improved?_

Related work includes blueprints (which is a similar concept for architecture)
and mathematical graph theory. Neither of those have the necessary tools for
specifically talking about electric circuits; blueprints are too concrete (the
exact size of an electric circuit is irrelevant to its operation, for example),
while graphs are too abstract (there are nodes and edges, but no way to
distinguish that a particular node is a transistor or that the different edges
of such a node are connected to it in different ways).

There is also, of course, the previous work of actual electric circuit diagrams,
which precisely solve this problem.

## The Project
This section examines whether the idea makes for a good CS 111 project.


### Suitability
_If someone were to work on this project, what percentage of their time would be
spent directly engaging in the **language** aspects of this project (e.g.,
making language design decisions), as opposed to "systems" aspects of the
project (e.g., implementing a complicated semantics that doesn't require a lot
of language design)?_

Almost all the time implementing this would be designing and describing the
language, since we would not build a computer program to process the language.
There would also be a lot of time spent learning about different circuit
components to better decide how to represent them.

### Scope
_How big an idea is this? How ambitious is this project?_

Depending on how it’s seen, this project would either be incredibly ambitious or
incredibly unambitious. If I were doing this project without reference to the
previous work, then part of it would be teaching people to read and understand
such diagrams when they didn’t already; if I were to use the previous work, I
would note that the entire project is already done.

### Benefits and drawbacks
_Why might this be a good idea for a project? Why might this not be a good idea 
project?_

This would make a bad project for this class. It has the drawbacks of being
unrelated to implementing DSLs on a computer, already having been done, and my
not knowing that much about different components of electric circuits. I can’t
see any redeeming features for its use as a project for this class.

