[[share build dependencies]]

Print an indented tree of dependencies of the given target.

###### Code

Use the script [[header]].

	set -x

	share-deptree: target=$1
	share-deptree: shift

	share-deptree: if test $# -gt 0; then
	share-deptree:     indent=$1
	share-deptree:     shift
	share-deptree: else
	share-deptree:     indent=""
	share-deptree: fi

	share-deptree: echo "${indent}${target}" | sed 's,#,	,g;s,␣, ,g'

	share-deptree: depdir=${target}.dep
	share-deptree: if test -d $depdir; then
	share-deptree:     for dep in $(cd ${depdir}; cat * | sed 's, ,␣,g' | sort); do

Change to the directory of the dependency and print its tree.

	share-deptree:         cwd=$(pwd)
	share-deptree:         cd "$(dirname '$dep')"
	share-deptree:         share-deptree "$(basename $dep)" "${indent}#"
	share-deptree:         cd $cwd
	share-deptree:     done
	share-deptree: fi

###### Usage

	host obsid lit 'lit=share-deptree.md' 'file=share-deptree'

Print a dependency tree.

	share deptree hello
	share deptree polylingual_hello.sh
