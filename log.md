###### Usage

	lit 'lit=log.md' 'file=log'

###### Code

Log in this format:

	$date_s	$0	$msg

Optionally execute command, and log output of command.

	log: set -eu
	log:error: set -o pipefail

Find all initial parameters with assignments, and add them to the environment (_cf_ [[argenv]]).

	log: for arg in "$@"; do
	log: 	if echo "$arg" | grep '=' >/dev/null 2>&1; then
	log: 		arg_name=$(echo $arg | sed 's =.*  ')
	log: 		arg_value=$(echo $arg | sed 's ^[^=]*=  ')
	log: 		eval ${arg_name}='${arg_value}'
	log: 		shift
	log: 	else
	log: 		break
	log: 	fi
	log: done

If the assignment parameters were stopped with `--`, then shift that out of the way.  We might want to use the remaining assignments as pass-through parameters.  (_cf_ [[argenv]])

	log: if test $# -gt 0; then
	log: 	if test "$1" = "--"; then
	log: 		shift
	log: 	fi
	log: fi

Determine how we are being called.  This relies on the [[literate/header.md]] exporting `$self` and `$selfdir`.

	log: selfdir=${selfdir:-.}
	log: calldir=${calldir:-$selfdir}
	log: self=${self:-sh}
	log: caller=${caller:-$self}
	log: logfile=${logfile-}

Log to a file if a path is given, otherwise log to [[standard error]].

	log: if test -n "$logfile"; then
	log:     echo "$(date +%Y%m%d.%H%M%S)	${calldir}/${caller}	$@" >>$logfile
	log: else
	log:     echo "$(date +%Y%m%d.%H%M%S)	${calldir}/${caller}	$@" >&2
	log: fi

Execute the command if requested (`exec=true`).

	log: exec=${exec:-false}
	log: if $exec; then
	log:     tmp=${tmp:-/tmp/${self}.$$}

Log output of the command if separately asked (`out=true`).

	log:     out=${out:-false}
	log:     if $out; then
	log:         "$@" >${tmp}.out 2>&1
	log:         if test -n "$logfile"; then
	log:             cat ${tmp}.out | sed "s,^,$(date +%Y%m%d.%H%M%S)	${calldir}/${caller}	${1}: ,g" >>$logfile
	log:         else
	log:             cat ${tmp}.out
	log:         fi
	log:     else
	log:         "$@"
	log:     fi
	log: fi
