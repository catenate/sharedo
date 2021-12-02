[[share build environment]]

List details of all known environment settings.

###### Code

Call script [[header]].

	share-lsenv: . header

	share-lsenv: for env in *.env; do
	share-lsenv:     for var in $(ls $env); do
	share-lsenv:         target=$(echo $env | sed 's \.env$  ')
	share-lsenv:         cat ${env}/${var} | sed "s,^,${target}	${var}	,"
	share-lsenv:     done
	share-lsenv: done >${tmp}

	share-lsenv: case $# in
	share-lsenv:     0)
	share-lsenv:         cat $tmp
	share-lsenv:         ;;
	share-lsenv:     1)
	share-lsenv:         awk "\$1 ~ /^$1$/{print \$0}" $tmp
	share-lsenv:         ;;
	share-lsenv: esac

###### Usage

	lit 'lit=share-lsenv.md' 'file=share-lsenv'

	share lsenv
