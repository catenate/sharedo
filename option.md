[[environment variable]]

Option sets the given variable with a null value, so the variable may be referenced by scripts which [[set -u]], and by [[test]] commands.  

###### Code

	option: eval $1=\${$1-}

Log the new option.

	option: log -- "$var=$(eval echo \$$var)"

###### Usage

	host obsid lit 'lit=option.md' 'file=option'

To change a variable in the calling script, it needs called in the calling script's environment, by the commands `.` or `source`.

	. option variable
