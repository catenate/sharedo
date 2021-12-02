[[share build dependencies]]

###### Code

Run the script [[header]].

	share-0dep: . header

Reset to 0 the checksums of all dependencies of a target.

	share-0dep: changed=false

	share-0dep: outfile=$1
	share-0dep: shift

	share-0dep: depdir=${outfile}.dep

	share-0dep: for oldsum in $(cd ${depdir}; ls); do
	share-0dep:     for dep in $(sed 's, ,␣,g' ${depdir}/${oldsum}); do

Change to the directory of the dependency and redo it with share-do.

	share-0dep:         newsum=0
	share-0dep:         if test "$oldsum" != "$newsum"; then

Update checksum for dependency.  Print checksum and filename of updated files.

	share-0dep:             changed=true
	share-0dep:             dep_quoted=$(echo "$dep" | sed 's / \\/ g;s,␣, ,g')
	share-0dep:             sed -i "/^${dep_quoted}$/d" ${depdir}/${oldsum}
	share-0dep:             echo "$dep" >>${depdir}/${newsum}
	share-0dep:             log "$newsum    $(echo $dep | sed 's,␣, ,g')"
	share-0dep:         fi
	share-0dep:     done
	share-0dep: done

The check dependency process updates the dependency checksum files with the current checksums of the files listed as dependencies of the given output file.  This script returns a true value if any dependency checksums were updated 0, false otherwise.

	share-0dep: if $changed; then
	share-0dep:     exit 0
	share-0dep: else
	share-0dep:     exit 1
	share-0dep: fi

###### Usage

	host obsid lit 'lit=share-0dep.md' 'file=share-0dep'

List all dependencies of a target.

	grep -n . polylingual_hello.sh.dep/* /dev/null

Set to 0 all checksums of dependencies of a target.

	host obsid share-0dep polylingual_hello.sh
