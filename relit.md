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

If a variant is specified, use it for all files.  The tag used for a variant may be coordinated across multiple files, and relit will extract that variant of all the files.  Given a variant, however, relit will not extract the default lines for a file, even if the variant does not exist in that file.  So all files handled by relit when a variant is specified should have lines for that variant wherever they have default lines.  #backlog

	relit: . default variant default

	relit: for lit in $lits; do
	relit: 	files=$(lit lit=$lit | sed -f $(dirname $0)/omit_interpreters.sed)

Don't try to extract files for tags with the names of interpreters.

	omit_interpreters.sed: /^bash$/d
	omit_interpreters.sed: /^perl$/d
	omit_interpreters.sed: /^python$/d
	omit_interpreters.sed: /^ruby$/d
	omit_interpreters.sed: /^sh$/d

Omit variants.  Extract the default variant, unless a variant is given.

	omit_interpreters.sed: /:/d

Extract each file tagged in the literate file.

	relit: 	for file in $files; do
	relit: 		log exec=true $echo lit lit=$lit file=$file variant=$variant
	relit: 	done
	relit: done

###### Usage

	lit 'lit=relit.md' 'file=omit_interpreters.sed'
	lit 'lit=relit.md' 'file=relit'

Test.

	host obsid relit 'test=true'
	host obsid relit 'echo=echo'

	host obsid relit 'test=true' share-library.md
	host obsid relit 'test=true' share-library.md share-add*.md

Actually extract files.

	host obsid relit
