[[share build dependencies]]

Remove all dependencies of a target, or one dependency of a target.

###### Code

	share-rmdep: case $# in
	share-rmdep:     1)
	share-rmdep:         rm -rf ${1}.dep
	share-rmdep:         ;;
	share-rmdep:     2)
	share-rmdep:         dep_quoted=$(echo "$2" | sed 's / \\/ g')
	share-rmdep:         file=$(grep -l "$dep_quoted" ${1}.dep/*)
	share-rmdep:         if test -n "$file"; then
	share-rmdep:             sed -i "/^${dep_quoted}$/d" $file
	share-rmdep:         fi
	share-rmdep:         ;;
	share-rmdep: esac

###### Usage

	host obsid lit 'lit=share-rmdep.md' 'file=share-rmdep'

Remove all dependencies of a target.

	host obsid share-rmdep polylingual_hello.sh

Remove a specific dependency of a target.

	host obsid share-rmdep polylingual_hello.sh '$PATH'

	host obsid share-rmdep polylingual_hello.sh always

	host obsid share-rmdep polylingual_hello.sh polylingual_hello.md

List all dependencies of a target.

	grep -n . polylingual_hello.sh.dep/* /dev/null
