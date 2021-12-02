[[share build rules]]

#backlog multiple outputs

enter on any of the set of targets

multiple identical .do files for each target, each of which generates all multiple output files

each do file copies the dep directory to the other files, so they are all considered up to date: [[share-cpdep]]

or, set up other targets as aliases of, or references to, one primary target, to avoid parallelism problems

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
