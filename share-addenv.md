[[share build environment]]

Add a [[environment variable]] for a [[target]].

###### Code

	set -x

	share-addenv: out=$1
	share-addenv: shift

	share-addenv: var=$1
	share-addenv: shift

	share-addenv: envdir=${out}.env
	share-addenv: if ! test -d $envdir; then
	share-addenv:     mkdir -p $envdir
	share-addenv: fi

	share-addenv: echo "$*" >${envdir}/$var

	share-addenv: share-rmdep $out "\$$var"
	share-addenv: share-adddep $out "\$$var"

###### Usage

	lit 'lit=share-addenv.md' 'file=share-addenv'

	share addenv hello.o CFLAGS ''
	share addenv hello.o CFLAGS -O2
	share addenv hello.o CFLAGS -O2 -Wall
	share addenv hello.o CFLAGS -Wall

	share rmdep hello.o
	share lsdep

	apply {file=$1; cat $file | sed 's,^,'^$file^': ,'} `{ls *.env}
	share lsenv
