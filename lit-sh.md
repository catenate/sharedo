[[literate program]]

Handle the common `run=sh` case, for self-running literate programs.

###### Code

	lit-sh: #!/usr/bin/env bash
	lit-sh: lit lit=$1 run=sh "$@"

###### Usage

	host obsid lit 'lit=lit-sh.md' 'file=lit-sh'

Test a self-running literate program.

	hello.lit: #!/usr/bin/env lit-sh
	hello.lit: Say hello.
	hello.lit: 	sh: echo hello

	host obsid lit 'lit=lit-sh.md' 'file=hello.lit'
	host obsid ./hello.lit
