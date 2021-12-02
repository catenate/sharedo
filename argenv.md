Parse initial assignment arguments as settings of named parameters, and store them in environment variables.

###### Code

Find all initial parameters with assignments, and add them to the environment.

	argenv: for arg in "$@"; do
	argenv: 	if echo "$arg" | grep '=' >/dev/null 2>&1; then
	argenv: 		arg_name=$(echo $arg | sed 's =.*  ')
	argenv: 		arg_value=$(echo $arg | sed 's ^[^=]*=  ')
	argenv: 		eval ${arg_name}='${arg_value}'
	argenv: 		shift
	argenv: 	else
	argenv: 		break
	argenv: 	fi
	argenv: done

If the assignment parameters were stopped with `--`, then shift that out of the way.  We might want to use the remaining assignments as pass-through parameters.

	argenv: if test $# -gt 0; then
	argenv: 	if test "$1" = "--"; then
	argenv: 		shift
	argenv: 	fi
	argenv: fi

If we're asked to set shell options, do so here.

	argenv: set=${set-}
	argenv: if test -n "$set"; then
	argenv: 	set $set
	argenv: fi

###### Usage

	host obsid lit 'lit=argenv.md' 'file=argenv'
