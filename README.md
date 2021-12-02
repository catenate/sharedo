Sharedo is an implementation in Bash of the build tool redo.  This implementation of redo provides small shell scripts which may be used to construct build systems from small shell scripts.  The scripts, and descriptions of how they are used, live in Markdown files which are literate programs.  The literate programs are extracted from the Markdown files with the tool `lit` (which lives in this repo).  I use Obsidian to browse between the Markdown files, so they use the syntax [[README]] to refer to each other.

###### Related Work

[Credo](https://github.com/catenate/credo) is an earlier, similar implementation of redo, written in the Inferno shell.
