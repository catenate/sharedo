Sharedo is an implementation in Bash of the recursive, general-purpose build tool [redo](https://redo.readthedocs.io/en/latest/).  This implementation of redo provides small shell scripts which may be used to construct build systems from small shell scripts.  The Bash scripts, and descriptions of how they are used, live in Markdown files which are literate programs.  The literate programs are extracted from the Markdown files with the tool `lit` (which also lives in this repo).  I use [Obsidian](https://obsidian.md/) to browse between the Markdown files, so they use the syntax [[README]] to refer to each other.

###### Related work

[Credo](https://github.com/catenate/credo) is an earlier, similar implementation of [redo](https://redo.readthedocs.io/en/latest/), written in the [Inferno shell](http://www.vitanuova.com/inferno/papers/sh.html).
