[[share build environment]]

Remove an [[environment variable]] for a [[target]].

###### Code

	share-rmenv: out=$1
	share-rmenv: shift

	share-rmenv: var=$1
	share-rmenv: shift

	share-rmenv: envdir=${out}.env
	share-rmenv: if test -d $envdir -a -f ${envdir}/$var; then
	share-rmenv:     rm -f ${envdir}/$var
	share-rmenv: fi

	share-rmenv: share-rmdep $out "\$$var"

###### Usage

	lit 'lit=share-rmenv.md' 'file=share-rmenv'

	share rmenv hello.o CFLAGS

	grep -n . *.dep/* /dev/null

	apply {file=$1; cat $file | sed 's,^,'^$file^': ,'} `{ls *.env}
	grep -n . *.env/* /dev/null
