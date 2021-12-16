[[share build dependencies]]

Return true if the checksum changed for any dependency, or a dependency did not yet have a checksum (share-adddep defaults the checksum of a new dependency to 0, which is not considered a checksum), or any dependency still has a checksum of 0.

###### Code

Use the script [[header]].

	share-chkdep: . header

	share-chkdep: changed=false

	share-chkdep: target=$1
	share-chkdep: shift

If there are no dependencies, then always rebuild the target.

	share-chkdep: depdir=${target}.dep

If there is a do script to create dependencies, run it.  #testing 

	share-chkdep: log exec=true $echo share-do ${depdir}

If there are still no dependencies, then just rebuild the target.

	share-chkdep: if ! test -d $depdir; then
	share-chkdep:     log -- "$target: no dependencies"
	share-chkdep:     exit 0
	share-chkdep: fi

Calculate the current checksum of each dependency with [[share-sum]], and determine whether it matches the current location of the dependency.  If not, remove the current entry for the file, and add a new one.

Map and parallelize this process, when it matters.  #backlog

	share-chkdep: for oldsum in $(cd ${depdir}; ls); do
	share-chkdep:     for dep in $(sed 's, ,␣,g' ${depdir}/${oldsum}); do

Change to the directory of the dependency and redo it with share-do.

For variables, use the format '$var', with no directory.  #testing 

	share-chkdep:         cwd=$(pwd)
	share-chkdep:         cd "$(dirname '$dep')"
	share-chkdep:         log exec=true $echo share-do "$(basename $dep)"
	share-chkdep:         cd $cwd

	$(cd "$(dirname '$dep')"; log exec=true $echo share-do "$(basename $dep)")

For a target like ${target}.dep, we still want to calculate a checksum (eg, a file with a list of files (_ie_ checksums) in the repo, or a list of the dependencies in those files).  How does this compare to just taking a checksum of the long listing of the dependency directory?  Should we add to the target's dependencies, a dependency on the dependency directory itself?   This would detect, for example, when we add an environment variable dependency, and at the same time calculate its checksum--though we solved this by always adding it with the checksum 0.  Should we add a dependency on the environment variable directory for the target?  Maybe just add these dependencies and see how it feels.  #backlog

	share-chkdep:         newsum=$(share-sum "$dep")
	share-chkdep:         if test "$oldsum" != "$newsum"; then

Update checksum for dependency.  Print checksum and filename of updated files.

	share-chkdep:             changed=true
	share-chkdep:             dep_quoted=$(echo "$dep" | sed 's / \\/ g;s,␣, ,g')
	share-chkdep:             sed -i "/^${dep_quoted}$/d" ${depdir}/${oldsum}
	share-chkdep:             echo "$dep" | sed 's,␣, ,g' >>${depdir}/${newsum}
	share-chkdep:             log "$newsum    $(echo $dep | sed 's,␣, ,g')"
	share-chkdep:         fi
	share-chkdep:     done
	share-chkdep: done

The check dependency process updates the dependency checksum files with the current checksums of the files listed as dependencies of the given output file.  This script returns a true value if any dependency checksums were updated (to tell [[share-do]] to proceed with a rebuild), false otherwise.

We could also check an argument to override this, and ignore the 0 file.

	share-chkdep: if test -s ${depdir}/0; then
	share-chkdep:     log -- "$target: unsummed dependencies"
	share-chkdep:     exit 0
	share-chkdep: fi

If the target does not exist, always rebuild it.  Or not, to allow non-file targets to only be built if their dependencies are out-of-date, and use `always` instead.

	share-chkdep: if ! test -f $target -o -d $target; then
	share-chkdep:     log -- "$target: file or directory not found"
	share-chkdep:     exit 0
	share-chkdep: fi

If any dependency changed, then rebuild the target.

	share-chkdep: if $changed; then
	share-chkdep:     log -- "$target: dependency changed"
	share-chkdep:     exit 0
	share-chkdep: fi

If none of the previous conditions hold, then do not rebuild the target.

	share-chkdep: log -- "$target: up-to-date"
	share-chkdep: exit 1

###### Usage

	host obsid lit 'lit=share-chkdep.md' 'file=share-chkdep'

List all dependencies of a target.

	grep -n . polylingual_hello.sh.dep/* /dev/null

Check dependencies.

	host obsid share-chkdep polylingual_hello.sh
