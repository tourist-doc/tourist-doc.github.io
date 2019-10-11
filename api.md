---
title: Server API
---

# Server API

The tourist binary includes a JSONRPC 2.0 server that operates over `stdio` and provides all of the
functionality that an editor needs to implement a tourist front-end. Interacting with the server
should be similar to interacting with an [LSP](https://langserver.org/) server.

Before describing the API, here are a few useful concepts to know about when working with tourist.


## Tour "Tracking"
By default, running `tourist serve` starts a server that doesn't know anything about tours on your
system[^1]. When you *open* a tour, tourist loads a tour file into memory and keeps it in its
"tracker". If the tour is modified, the changes are not persisted to disk until the tour is *saved*.

The tracker also keeps track of some metadata about the tour. When you open a tour, have the option
to open it for reading or for editing. If open for reading, the tour cannot be modified, and any
edit operations will fail.


## Versioning and *Refresh*
Each tour keeps track of a list of git repositories, each with an associated commit. When a stop is
added to a tour from a new repository, the HEAD of the repository is set as the current commit. As
the repository evolves, the tour will become "out of date". Generally, if a tour is out of date, you
should do one of two things:
- If you wish to **view** the tour, you can ask tourist to check out the appropriate commits for
  you. This will go to each repository in the tour and run a `git checkout`. Note that this
  operation will fail if the repositories have changes.
- If you wish to **edit** the tour, you should ask tourist to *refresh*. This will run diffs against
  each repository and move the tour stops based on the way the files have changed. For example, if a
  tour points to line 2 of the following file:
  ```
  1 foo
  2 bar
  3 baz
  ```
  and the file is updated to
  ```
  1 foo
  2 NEW
  3 NEW
  4 bar
  5 baz
  ```
  running a refresh will move the stop to line 4.

If the line a stop points to is changed, the it will be impossible to correctly refresh the tour.
The stop will be marked "broken". In this mode, tourist will fall back to showing the last known
location of the stop, but that location might be semantically incorrect. To fix a broken stop,
simply move the stop to the correct line.

Technically speaking, most operations are actually allowed on an out of date tour. The only
operations that will fail explicitly are ones that try to create or move a tour stop. Still, it is
best for users (and editor plugins, when possible) to keep tours up to date.


##

----

[^1]: You can configure tourist to look for tours in one or more directories on startup. You can
    read more on the [configuration]() page.
