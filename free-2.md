# Free project 2


## The user and a language
This section describes who the project would serve and why a language might be a
good way to meet their needs.


### What's the need?
_What need is met by your idea? Who are you helping? What is that person's
experience like now? What would their experience be like if you could help 
them?_

Sometimes, people need to download information from the Internet. For simple
cases, this can be done in a web browser; for more complicated cases, programs
like [`wget`](https://www.gnu.org/software/wget/) or
[`curl`](http://curl.haxx.se/) can be used. However, even these solutions can be
inappropriate in many cases.

The `wget` program allows several advanced downloading features. One of these is
the ability to download pages that are linked to from an original page, and to
recurse in this downloading. However, this recursion is too broad: often, one
only wants to download links from the “body” of a web page, not from various
headers; the existing options of only downloading from a single domain don’t
help. For example, if one wanted to download the
[Scala Guides and Overviews](http://docs.scala-lang.org/overviews/), then `wget`
wouldn’t be the appropriate tool all by itself because, even with all the
options to restrict what gets downloaded, you’d still get irrelevant pages like
the [index of tutorials](http://docs.scala-lang.org/tutorials/).

Other times, one wants to download information repeatedly to check for and
store changes. For example, some of those guides are about experimental features
of Scala, and might change over time. The Scala source itself
[is on GitHub](https://github.com/scala/scala), but as far as I can tell those
guides are not. Somebody might want to track changes to those guides, store
those changes in a Git repository, and compile each change into a versioned
ebook. That is practically impossible to do with `wget` now.

### Why a language?
_Why is a DSL appropriate for your user(s)? How does it address the need?_

A domain-specific language would allow somebody to write a program that
downloads files from the Web, keeps track of changes, and does something with
those changes. Because the program might need to be run several times and might
need modification, a DSL is the right tool for the job; an application would not
allow as easy saving, re-running, or modification. Furthermore, in most cases
only a few features are necessary for any given program; having to sort through
many different options as with a GUI application would slow the user down.

### Why you?
_What excites you about this idea? How did you come up with it?_

I have needed this program for a long time. The first time I can remember where
I wanted it was when trying to download entire books from
[WikiSource](https://en.wikisource.org/wiki/Main_Page), back when this was
harder than it currently is (although, as far as I can tell, it still isn’t easy
to download books that are more than one page). Other uses I’ve wanted it for
were making up-to-date ebooks out of fiction published serial online (both fan-
and original fiction) and archiving changing market information for a game with
a stock-market like component.

There are other uses I can remember wanting something like this for, but I can’t
call them to mind at the moment. Some of the above uses are still things I want,
though, and I’m sure I’ll think of others as time goes on.

I didn’t realize that a downloading DSL was the right tool for this problem
until I was trying to come up with project ideas for this class; I usually
attempted to solve the problem individually for each use case, gave up partway
through because there’s a lot of boiler plate and I moved on to other projects,
and forgetting the similarities to other problems I’d had before.

### Domain
_Describe the project's domain in five words._

Downloading and processing complex websites

### Interface (syntax)
_How might the user interact with the language? What does programming look 
like? Why is this the right way to interact with the problem domain?_ 

This would probably be declarative, like a configuration file. Something like
the following might work for downloading the scala guides daily, downloading the
list weekly to check for new guides, making an ebook with all the guides (using
the external program `process_scala_toc` to convert the ToC into a format that
can be read by the ebook generator, extracting the example code, and storing all
the relevant stuff in a Git repository.

```
Settings
	RelativeLinks
	Images = imgs
	NoCSS
Page ScalaToC
	Page    = http://docs.scala-lang.org/overviews/
	# CSS paths would work in addition to XPath
	Content = /html/body/div[1]/div[2]/div/div/div/div/div[1]
	Links   = ScalaGuides
	Repeat  = 1 week
Category ScalaGuides
	Title   = /html/body/div[2]/div/div[2]/h1
	Content = /html/body/div[2]/div/div[4]
	Repeat  = 1 day
Ebook ScalaGuides
	ToC   = !process_scala_toc ScalaToC
	Pages = ScalaGuides
	Other = imgs
	Out   = epub, mobi
!process_scala_examples
	Input     = ScalaGuides
	OutputDir = Examples
Git
	ScalaToC, ScalaGuides, imgs, examples
```

Some potential problems with this syntax:

* External programs which aren’t built into the language are called differently
* XPath isn’t really readable sometimes, and in this case especially fragile
* It requires writing external programs to do any processing not built-in
* There is currently no way to import a lot of similar settings if the same
  sorts of things need to be done for multiple files

### Operation (semantics)
_What might happen when a program runs? How does a program interact with the
user? What kinds of errors might occur, and how might they be communicated to
the user?_

The program would be read and parsed as a declarative structure. Each of the
sections is a list of actions, either to download from the web or do some
processing on the downloaded files. Anything that had an external dependency,
like a webpage, would either be run right away or scheduled as appropriate;
anything that did not have external dependencies would be run whenever all the
inputs were available and one of them updated.

Alternatively, instead of scheduling everything itself, it might be better to
compile the language to a set of `cron` jobs or other forms of scheduled task.

### Expressiveness
_What should be easy to do in this language? What should be possible, but
difficult? What should be impossible or very difficult?_

It should be easy to download webpages, complex networks of webpages, and
related resources. Common processing, like extracting only a part of the webpage
or bundling a collection of webpages into an ebook, should be easy; custom
processing, like extracting code from a webpage, should be possible. Since this
program allows shell escapes, nothing is truly impossible, but anything that
requires complex control flow is difficult.

### Related work
_Are there any other DSLs in this domain? If not, describe how you know there
aren't and conjecture why not. If so, describe them and provide links. How well 
do they address the need? Are there any particularly admirable qualities of the
language? Are there parts of the language you think could be improved?_

As mentioned above, this is similar in many ways to `wget` and `curl`, which can
be viewed as DSLs (although command lines generally make horrible languages). I
know that if there are any that do the processing I want, they are *very
difficult* to find, because I have *been looking* for years. This could be
because there isn’t as much in common between these tasks as I think and this
project is doomed, it could be that everybody rolls their own custom solutions
and never made something general, or it could be that I have been looking in the
wrong places.

Similar domains include build systems, which map changing inputs to outputs, but
those don’t generally except inputs on the Web and they don’t generally support
the kinds of archiving or combining that web crawling would often need.

## The Project
This section examines whether the idea makes for a good CS 111 project.


### Suitability
_If someone were to work on this project, what percentage of their time would be
spent directly engaging in the **language** aspects of this project (e.g.,
making language design decisions), as opposed to "systems" aspects of the
project (e.g., implementing a complicated semantics that doesn't require a lot
of language design)?_

To make the system really useable, a large amount of time would need to be spent
on the semantics, unfortunately. If everything could be handled via shell
escapes—especially if shell escapes were made to look like part of the
language—then writing the language would be much easier but using it would
require more external tools.

### Scope
_How big an idea is this? How ambitious is this project?_

Getting the semantics of this project up to an easy-to-use level is ambitious.
Building the language itself is probably not particularly ambitious, but that’s
difficult to judge. The language can be extended pretty far, but it’s hard to
tell how much of that would be library vs. DSL, and how much would be complex
semantics vs. language design.

### Benefits and drawbacks
_Why might this be a good idea for a project? Why might this not be a good idea 
project?_

This could be a good idea for a project because it would produce something
useful, and because it would be a good DSL. It might not be a good idea for a
project if it turned out to be too semantics-heavy.
