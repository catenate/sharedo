[[share build environment]]

###### Code

	share-setenv: target=$1
	share-setenv: shift
	share-setenv: envdir=${target}.env

If there are no environment variables to incorporate into the current environment, don't do anything.  But don't exit, since this is called in the context of [[share-do]], so will exit the calling script.

	share-setenv: if test -d "$envdir"; then
	share-setenv:     for var in $(cd ${target}.env; ls); do
	share-setenv:         val="$(cd ${target}.env; cat $var)"
	share-setenv:         eval "export $var='$val'"
	share-setenv:         log -- "$var=$val"
	share-setenv:     done
	share-setenv: fi

###### Usage

In [[share-do]], run `.` [[share-setenv]] `$out`.  For files in `${out}.env`, set the [[environment variable]] named by the file to the contents of the file, with eval in the current environment.  #backlog

Export these variables to dependent targets?  #backlog 

	lit 'lit=share-setenv.md' 'file=share-setenv'
	
	share setenv hello.o
