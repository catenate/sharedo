[[share build dependencies]]

Compute the checksum of the given dependency.

###### Code

	set -x

If the file is a variable ($var), echo the current value of the variable, and take its checksum with [[md5sum]].

	share-sum: if echo "$*" | grep '^\$' >/dev/null 2>&1; then
	share-sum:     eval "echo $*" | md5sum
	share-sum: else

If the file is a command (has a space), catenate the standard output of the command, and take its checksum with [[md5sum]].

	share-sum:     if echo "$*" | grep '␣' >/dev/null 2>&1; then
	share-sum:         eval $(echo $* | sed 's,␣, ,g') | md5sum
	share-sum:     else

If the file does not exist, then keep it unknown.

	share-sum:         if test -r "$*"; then
	share-sum:             cat "$*" | md5sum
	share-sum:         else
	share-sum:             echo 0
	share-sum:         fi
	share-sum:     fi
	share-sum: fi

	share-sum: exit 0

###### Usage

	lit 'lit=share-sum.md' 'file=share-sum'

	share sum polylingual_hello.md

	share sum '$PATH'

	share sum always

	share sum gcc --version
	share sum gcc␣--version
