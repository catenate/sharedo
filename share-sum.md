[[share build dependencies]]

Compute the checksum of the given dependency.

###### Code

	share-sum:debug: set -x

If the file is a variable ($var), echo the current value of the variable, and take its checksum with [[md5sum]].

	share-sum: if echo "$*" | grep '^\$' >/dev/null 2>&1; then
	share-sum:     eval "echo $*" | md5sum
	share-sum:     exit 0
	share-sum: fi

If the file is a command (has a space), catenate the standard output of the command, and take its checksum with [[md5sum]].

	share-sum: if echo "$*" | grep '␣' >/dev/null 2>&1; then
	share-sum:     eval $(echo $* | sed 's,␣, ,g') | md5sum
	share-sum:     exit 0
	share-sum: fi

If we are given a target that is a directory, then we want to calculate the checksum of the directory based on the long list of files in the directory.  This will detect date changes, size changes, and ownership and permissions changes of the files, as well as added, renamed, or removed files, without calculating checksums of each of the files.  #testing

	share-sum: if test -d "$*"; then
	share-sum:     cd "$*"
	share-sum:     ls -l | md5sum
	share-sum:     exit 0
	share-sum: fi

Just take the checksum if a the file is readable.

	share-sum: if test -r "$*"; then
	share-sum:     cat "$*" | md5sum
	share-sum:     exit 0
	share-sum: fi

Otherwise, keep the checksum unknown.

	share-sum: echo 0
	share-sum: exit 0

###### Usage

	lit 'lit=share-sum.md' 'file=share-sum'
	lit 'lit=share-sum.md' 'file=share-sum' 'variant=debug'

	share sum polylingual_hello.md

	share sum '$PATH'

	share sum always

	share sum 'hello.o.env'
	cd hello.o.env; host ls -l | host md5sum

	share sum gcc --version
	share sum gcc␣--version
