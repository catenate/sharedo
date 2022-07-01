[[environment variable]]

Require returns an error if the given variable is not set or is empty.

###### Code

Log the value of the required variable.

	require: log -- "$1=$(eval echo \$$1)"

Check whether it has a value.

	require: eval "test -n \"\$$1\""

###### Usage

	host obsid lit 'lit=require.md' 'file=require'

To change a variable in the calling script, it needs called in the calling script's environment, by the commands `.` or `source`.

	. require variable
