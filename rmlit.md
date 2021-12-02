[[literate program]]

Clean up files derived from literate programs.

###### Code

	rmlit: echo rm -f $(lslit)

###### Usage

	host obsid lit 'lit=rmlit.md' 'file=rmlit'

	host obsid rmlit
