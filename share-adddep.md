[[share build dependencies]]

Add a [[dependency]] for a [[target]].

###### Code

	share-adddep: out=$1
	share-adddep: shift

	share-adddep: dep=$1
	share-adddep: shift

	share-adddep: depdir=${out}.dep
	share-adddep: if ! test -d $depdir; then
	share-adddep:     mkdir -p $depdir
	share-adddep: fi

Check whether the [[dependency]] is already captured, or add a new entry for the dependency.

	share-adddep: if ! grep "$dep" ${depdir}/* >/dev/null 2>&1; then
	share-adddep:     echo "$dep" >>${depdir}/0
	share-adddep: fi

###### Usage

	lit 'lit=share-adddep.md' 'file=share-adddep'

	share adddep polylingual_hello.sh polylingual_hello.md

The [[dependency]] [[always]] (or any dependency that is not an actual file) will remain 0, since there is no file to create a checksum.  These dependencies will force the target to always compile.  These dependencies are also useful to represent dependencies which are not understood, or are out of the scope of the build system.

Always build a target.

	share adddep polylingual_hello.sh always

Capture dependencies on environment variables.

	share adddep polylingual_hello.sh '$PATH'

List all dependencies of a target.

	grep -n . polylingual_hello.sh.dep/* /dev/null
