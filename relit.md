[[literate program]]

Regenerate files from all literate programs in this directory.

###### Code

Use the script [[header]].

	relit: . header

	relit: lits=$(ls *.md | grep -v ' ')

	relit: for lit in $lits; do
	relit: 	files=$(lit lit=$lit | sed -f $(dirname $0)/omit_interpreters.sed)
	relit: 	for file in $files; do
	relit: 		log exec=true $echo lit lit=$lit file=$file
	relit: 	done
	relit: done

Don't try to extract files for tags with the names of interpreters.

	omit_interpreters.sed: /^perl$/d
	omit_interpreters.sed: /^python$/d
	omit_interpreters.sed: /^ruby$/d
	omit_interpreters.sed: /^sh$/d

###### Usage

	host obsid lit 'lit=relit.md' 'file=omit_interpreters.sed'
	host obsid lit 'lit=relit.md' 'file=relit'

Test.

	host obsid relit 'test=true'
	host obsid relit 'echo=echo'

Actually extract files.

	host obsid relit
