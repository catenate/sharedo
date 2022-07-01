#testing 

### pastout

#### Code

Run the script [[header]] in the environment of the calling script.

	pastout: . header

Find `$test`, the name of the test, either given as a named parameter, or extracted from `$1`, the name of the markdown file of the test to run.

	pastout: if test $# -gt 0; then
	pastout:     . default case $1
	pastout: fi
	pastout: case=$(echo $case | sed 's \.test\.md$  ')
	pastout: . require case

Define test files in `${case}.test.md`:
- In the Input section, `${case}.in/` contains files before the test is run
- In the Output section, `${case}*.test.do` contains the script to run to execute the test, and create files in `${case}*/`.

Run [[relit]] to extract the files.

	pastout: relit ${case}.test.md

Run all the do scripts defined by the markdown file (using [[lit]] to find them), to create the output files for each variant.

	pastout: for do in $(lit lit=${case}.test.md | sed -n 's \.do$  p'); do
	pastout:     share-do $do
	pastout: done
	pastout:mapex: mapex 'share do $x' $(lit lit=${case}.test.md | sed -n 's \.do$  p')

Check whether the tree of files in the out tree matches the tree of files in the past tree.  This is redundant with a full scan of all the files, but it's a lot quicker, so will exit sooner if the file set is not identical.

	pastout:scan: outsum=$(cd out; find | sort | md5sum)
	pastout:scan: pastsum=$(cd past; find | sort | md5sum)
	pastout:scan: test "$outsum" = "$pastsum"

Compare the checksum of every file in the past tree to the corresponding file in the out tree.

	pastout:scan: outsum=$(cd out; find | sort | xargs md5sum | md5sum)
	pastout:scan: pastsum=$(cd past; find | sort | xargs md5sum | md5sum)
	pastout:scan: test "$outsum" = "$pastsum"

If there are no differences, then the test passes.  Otherwise, the test fails.

Alternatively, the script `test-${case}.out.do` also checks for consistency between the out directory and the past directory, so it can handle binary files that are awkward to extract from literate programs.

#### Usage

	lit 'lit=pastout.md' 'file=pastout'

### regress

#### Code

regress is a second, separate script that finds all the `test-*.md` files, runs `pastout` against each of them, and returns true if they all succeed.

	regress: mapex 'pastout $x' test-*.md

#### Usage

	lit 'lit=pastout.md' 'file=regress'
