---
title: Tourist
---

Tourist is an innovative new approach communicating about code. While traditional forms of
documentation have their uses, they fall down in some key areas:
- Code comments clutter the code and potentially obscure the logic. They are sometimes useful for
  pointing out technical details, but are rarely the right place for higher-level documentation.
- Wikis and other long-form documentation provide a nice high-level picture, but they don't provide
  a robust way to link that picture to the code itself. In addition, these forms of documentation
  often get out of sync with the code.
- API documentation is great for *users* of the code, but should not be cluttered with technical
  details about the underlying algorithms.

Tourist solves these problems with editor-integrated tools for writing and viewing **high level**,
**technical** documentation that is **linked to the code itself**. Tourist also helps you to keep
your documentation up to date with **automated git integration**.

## What is Tourist?
In software engineering, it is often natural to want to give someone a "tour" of some code. Maybe a
senior developer wants to show a new hire the code that they'll be working on. Maybe you have a code
change in mind, but want a second set of eyes on the places you're planning on making changes.
Traditionally, these communications would happen in real time, with one person pointing to the code,
and the other nodding along as technical details fly their way. Tourist improves on this notion of a
"code tour", replacing the real-time interaction with a concrete artifact that can be used as
documentation as long as needed.

Currently, Tourist exists as an [extension for Visual Studio Code](), and as a [package for
Emacs](). Both tools provide intuitive ways to create, update, and view code tours.

## What is a tour?
At its core, a tour is just a list of **code locations** along with **markdown documentation**. Each
of these is called a *tour stop*.

As you view a tour, Tourist leads you through the stop, taking you to each location and displaying
the relevant prose on the side.

[DEMO]

When you want to add stops to a tour, you can do it from right in the code. Both editors provide
idiomatic ways to create a stop wherever you happen to be in your codebase.

[DEMO]

## Keeping Up with Changes
Code is never static, so Tourist was designed with changes in mind. Tourist requires that any code
being toured live in one or more [git](https://git-scm.com/) repositories. The tour keeps track of
the version of the code when it was written, and Tourist provides easy commands for checking out the
version of the code that the original writer intended to document. This means that while it is
possible that a tour might not be completely up to date, it won't lie (as comments sometimes do)
about the version of the code being referenced.

For example, imagine a repository with some git history:
```
HEAD -> 0ab1    Added a really cool feature, but I haven't documented it yet.
        2f98    Not such interesting code here, but I made some changes.
        aa44    Write some really cool code! I'd better document it with Tourist.
        ...
```

If a tour is written for commit `aa44`, and then two
more commits (`2f98` and `0ab1`) are made on top of it, Tourist will recommend that the user check
out `aa44` before viewing the tour.

As a documentation maintainer, Tourist also makes it easy to update a tour to a new commit. When
asked, Tourist can automatically determine if a tour stop has moved, and, if so, it updates the tour
automatically. If Tourist can't find the stop anymore (likely because the code has changed
significantly), it invalidates the stop and asks the tour writer to touch up their documentation.
This mechanism makes it easy to stay on top of stale docs.

In the example above, if the new commits don't significantly change the code that the tour
documents, the tour can automatically be updated to `0ab1`.
