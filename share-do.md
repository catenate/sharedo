Minimal [[bash]] version of the [[redo]] [[build automation tool]].
- about [[share build rules]]
- about [[share build dependencies]]

Use an existing executable `.do` file to build a target, if any dependency changed, had its checksum calculated for the first time, or could not get a checksum.

###### Code

	share-do: . header

	share-do: target=$1
	share-do: shift

If we're not already logging to a file, log to the do log for this target.

	share-do: logfile=${logfile-}
	share-do: if test -z "$logfile"; then
	share-do:     export logfile=${target}.do.log
	share-do: fi

No real value to logging the target, since [[share-chkdep]] lets us know when it calls share-do again, and the log of [[flock]] lets us know about this target.

	log -- "target=$target"

	share-do: base=$(echo $target | sed 's \.[^.]\+$  ')

If there's a ${target}.do.do file, run it to (re-)create ${target}.do.

	share-do: if test -x ${target}.do.do; then
	share-do:     log exec=true $echo share-do ${base}.do
	share-do: fi

	share-do: if test -x ${target}.do; then
	share-do:     if share-chkdep $target; then

Import the environment set for this target.

	share-do:         . share-setenv $target

Use [[flock]] to create a lock for the file under construction.  This ensures that only one share-do process at a time runs the script to create the target.

	share-do:         log exec=true $echo flock ${target}.lock -c "sh -x ${target}.do $base" 
	share-do:     fi
	share-do: else
	share-do:     log -- "$target: no script"
	share-do: fi

###### Usage

	lit 'lit=share-do.md' 'file=share-do'

	share do 'test=true' polylingual_hello.sh

	share do 'set=-x' polylingual_hello.sh

	share do polylingual_hello.sh
