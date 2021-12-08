[[share build environment]]

Add a [[environment variable]] for a [[target]].

###### Code

	set -x

	share-addenv: target=$1
	share-addenv: shift

	share-addenv: var=$1
	share-addenv: shift

	share-addenv: envdir=${target}.env
	share-addenv: if ! test -d $envdir; then
	share-addenv:     mkdir -p $envdir
	share-addenv: fi

Remove existing dependency and add it again with checksum 0, since the value may have changed.  The checksum needs to be 0 so the target knows it's out-of-date with respect to the new value.

	share-addenv: share-rmdep $target "\$$var"
	share-addenv: share-adddep $target "\$$var"

Might be better to check the new value against the current value for this target, and only update the checksum to 0 if it has changed.  To do this, we need to take the checksum of the new value, and compare it against the currently-recorded checksum for this variable for this target, if there is one.  #backlog  

	share-addenv: echo "$*" >${envdir}/$var

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
