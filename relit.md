[[literate program]]

Regenerate files from all literate programs in this directory, or from given files.

###### Code

Use the script [[header]].

	relit: . header

	relit: case $# in
	relit:     0)
	relit:         lits=$(ls *.md | grep -v ' ')
	relit:         ;;
	relit:     *)
	relit:         lits="$@"
	relit:         ;;
	relit: esac

	relit: for lit in $lits; do
	relit: 	files=$(lit lit=$lit | sed -f $(dirname $0)/omit_interpreters.sed)

Don't try to extract files for tags with the names of interpreters.

	omit_interpreters.sed: /^bash$/d
	omit_interpreters.sed: /^perl$/d
	omit_interpreters.sed: /^python$/d
	omit_interpreters.sed: /^ruby$/d
	omit_interpreters.sed: /^sh$/d

Extract each file tagged in the literate file.

	relit: 	for file in $files; do
	relit: 		log exec=true $echo lit lit=$lit file=$file
	relit: 	done
	relit: done

###### Usage

	host obsid lit 'lit=relit.md' 'file=omit_interpreters.sed'
	host obsid lit 'lit=relit.md' 'file=relit'

Test.

	host obsid relit 'test=true'
	host obsid relit 'echo=echo'

	host obsid relit 'test=true' share-library.md
	host obsid relit 'test=true' share-library.md share-add*.md

Actually extract files.

	host obsid relit
