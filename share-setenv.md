[[share build environment]]

###### Code

	share-setenv: target=$1
	share-setenv: shift
	share-setenv: envdir=${target}.env

If there are no environment variables to incorporate into the current environment, don't do anything.  But don't exit, since this is called in the context of [[share-do]], so will exit the calling script.

	share-setenv: if test -d "$envdir"; then
	share-setenv:     for var in $(cd ${target}.env; ls); do
	share-setenv:         val="$(cd ${target}.env; cat $var)"

Export these variables to dependent targets?  On the one hand, it would be easier to set something once, and have it ripple through the build.  On the other, this may be going the wrong way (from most-dependent object up to least-dependent), may set values which are different for different dependencies (_eg_ flags and definitions might vary from one source file to another), and since we have the ability to auto-generate dependencies on flags from the command line, we might as well use that to explicitly set many dependencies on the same variable, rather than ripple through definitions, in some cases have to block them, and deal with figuring out when to mask previous definitions and when to add them.

	share-setenv:         eval "export $var='$val'"
	share-setenv:         log -- "$var=$val"
	share-setenv:     done
	share-setenv: fi

###### Usage

In [[share-do]], run `.` [[share-setenv]] `$out`.  For files in `${out}.env`, set the [[environment variable]] named by the file to the contents of the file, with eval in the current environment.  #testing

	lit 'lit=share-setenv.md' 'file=share-setenv'
	
	share setenv hello.o
