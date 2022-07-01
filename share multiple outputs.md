[[share build rules]]

#backlog multiple outputs

enter on any of the set of targets

multiple identical .do files for each target, each of which generates all multiple output files

need one lock file name for all dup targets (or to not compete)

expand locked section to include chkdep, since other target has already checked deps, and is just waiting to execute, when first target updates them?  check again?  kill waiting process since it will be redundant?  store the PID of a multiout target to make it easy to kill

each do file copies the dep directory to the other files, so they are all considered up to date: [[share-cpdep]]

or, set up other targets as aliases of, or references to, one primary target (possibly different from all the actual files created by the multiout target), to avoid parallelism problems

    share-output: primary=$1
    share-output: shift

    share-output: for secondary in "$@"; do
    share-output:     share-lib tool=noop file=$secondary
    share-output:     share-adddep $secondary $primary
    share-output: done

secondary target file: do file is a no-op (noop...do); depends on primary target file.  When secondary target is requested, after primary target is built, [[share-chkdep]] will just update seconary's checksum of primary.

create these from the library, since they will be a common pattern for multiple output files
- [[share-lib]] multiout do do.do secondary
	- cp multiout.do.do.do secondary.do.do
		- `echo "share-do $2" >${1}.do`
	- run secondary.do.do secondary primary
		- create secondary.do
			- `share-do primary`
- [[share-lib]] multiout dep dep.do secondary
	- secondary.dep.do
		- `share-adddep secondary always`

---

for multiple outputs:
- first one is normal
- others depend on the first one, copy deps, and just use chkdep to bring deps up to date