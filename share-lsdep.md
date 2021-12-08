[[share build dependencies]]

List details of all known dependencies.

###### Code

Call script [[header]].

	share-lsdep: . header

	share-lsdep: for dep in *.dep; do
	share-lsdep: 	if test -d $dep; then
	share-lsdep:         for sum in $(ls $dep); do
	share-lsdep:             target=$(echo $dep | sed 's \.dep$  ')
	share-lsdep:             cat ${dep}/${sum} | sed "s,^,${sum}	${target}	,"
	share-lsdep:         done
	share-lsdep:     fi
	share-lsdep: done | sort -k 2,3 >${tmp}

	share-lsdep: case $# in

With no parameters, list all dependencies.

	share-lsdep:     0)
	share-lsdep:         cat $tmp
	share-lsdep:         ;;

With one parameter, list dependencies of the given target.

	share-lsdep:     1)
	share-lsdep:         awk "\$2 ~ /^$1$/{print \$0}" $tmp
	share-lsdep:         ;;

With two parameters, list dependencies of the given target, which match the given pattern.

	share-lsdep:     2)
	share-lsdep:         awk "\$2 ~ /^$1$/{print \$0}" $tmp | grep "$2"
	share-lsdep:         ;;
	share-lsdep: esac

###### Usage

	lit 'lit=share-lsdep.md' 'file=share-lsdep'

	share lsdep
	share lsdep polylingual_hello.sh
	share lsdep nope

	share lsdep hello
	share lsdep hello.o
	share lsdep hello '\.o$'
	share lsdep hello '\.o$' | host awk '{print $3}'
	share lsdep hello.o '\.c'
