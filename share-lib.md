[[share build rules]]

Copy the requested translation from the library to a `.do` file for the target.

###### Code

Run the script [[header]].

	share-lib: . header

	share-lib: . default infile ${file}.${in}
	share-lib: . option out

	share-lib: if test -n "$out"; then
	share-lib: 	. default outfile ${file}.${out}
	share-lib: 	. default outdo ${outfile}.do
	share-lib: else
	share-lib: 	. default outfile ${file}
	share-lib: 	. default outdo ${outfile}.do
	share-lib: fi

Don't overwrite `.do` scripts that already exist and are executable.  This is more annoying than helpful.

	if ! test -x $outdo; then

	share-lib: log exec=true $echo cp ${selfdir}/${tool}.${in}.${out}.do $outdo
	share-lib: log exec=true $echo chmod +x $outdo
	share-lib: log exec=true $echo share-adddep $outfile $outdo

	fi

###### Usage

	lit 'lit=share-lib.md' 'file=share-lib'

Clean up output file and recreate it.

	rm polylingual_hello.sh.do

	host obsid share-lib 'tool=lit' 'in=md' 'out=sh' 'file=polylingual_hello'

Show what's going on in the script.

	host obsid share-lib 'tool=lit' 'in=md' 'out=sh' 'file=polylingual_hello' 'set=-x'

Test the script.

	host obsid share-lib 'tool=lit' 'in=md' 'out=sh' 'file=polylingual_hello' 'test=true'
