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
	share-lsdep:     0)
	share-lsdep:         cat $tmp
	share-lsdep:         ;;
	share-lsdep:     1)
	share-lsdep:         awk "\$2 ~ /^$1$/{print \$0}" $tmp
	share-lsdep:         ;;
	share-lsdep: esac

###### Usage

	lit 'lit=share-lsdep.md' 'file=share-lsdep'

	share lsdep
	share lsdep polylingual_hello.sh
	share lsdep nope
