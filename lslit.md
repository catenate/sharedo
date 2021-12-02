[[literate program]]

List files derived from literate programs.

###### Code

	lslit: grep -l '^# DO NOT EDIT' * 2>/dev/null
	lslit: exit 0

###### Usage

	host obsid lit 'lit=lslit.md' 'file=lslit'

	host obsid lslit
