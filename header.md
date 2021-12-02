###### Usage

In a script, call this with

	. header

Extract this literate program.

	host obsid lit 'lit=header.md' 'file=header'

###### Code

Define general shell options.

	header: set -eu
	header: set -o pipefail

Let the script and any subscripts know its name and location.

	header: export selfdir=$(dirname $0)
	header: export self=$(basename $0 | sed 's \.sh$  ')

Set up a path to temporary files created by the calling script.

	header: export tmp=/tmp/${self}.$$

Delete the temporary files created by this script.

	header: trap 'rm -rf ${tmp}*' EXIT HUP INT QUIT TERM

Set up scripts to use [[named parameter]]s.

	header: . argenv

Echo commands if we are in testing mode.

	header: echo=${echo-}
	header: test=${test:-false}
	header: if $test; then
	header: 	echo=echo
	header: fi
