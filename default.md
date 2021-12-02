[[environment variable]]

Default sets the given variable to the given value, if the variable is currently unset, or if the current value of the variable is empty. 

###### Code

	default: var=$1
	default: shift

	default: eval $var=\${$var:-$*}

Log the new default.

	default: log -- "$var=$(eval echo \$$var)"

###### Usage

	host obsid lit 'lit=default.md' 'file=default'

To change a variable in the calling script, it needs called in the calling script's environment, by the commands `.` or `source`.

	. default variable value
