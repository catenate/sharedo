##### Compile through an object file to an executable

Clean up.

	share rmdo 'test=true'
	share rmdo

	share rmdep aloha
	share rmdep aloha.o
	share rmdep aloha.o.dep
	share rmdep helloha
	share rmdep helloha.o.dep

	share rmenv hello.o CFLAGS

List all dependencies.

	share lsdep
	share deptree hello

List all environment settings.

	share lsenv

List commands.

	share lscmd hello
	share lscmd hello.do
	share lscmd hello.o
	share lscmd hello.o.dep

###### Object file

Create build script.

	share lib 'tool=gcc' 'in=c' 'out=o' 'file=hello'
	share catdo hello.o

Remove all dependencies for this object.

	share rmdep hello.o

Add dependencies.

	share rmdep hello.o.dep

	share lib 'tool=gcc' 'in=c' 'out=o.dep' 'file=hello'
	share catdo hello.o.dep

	share do 'set=-x' hello.o.dep
	share do hello.o.dep

Don't need this anymore, since it is taken care of, by the do script to generate dependencies of the object file.

	#share adddep hello.o hello.h

Set checksums of dependencies.  If [[share-chkdep.md]] is run by itself, then it brings the dependency checksums up-to-date.  This could be a problem, if the target exists, since the dependencies are up-to-date, but the target was not rebuilt.  In this case, [[share build dependencies]] has no way of knowing that the target is out-of-date with respect to its up-to-date dependencies.  To get out of this situation, delete the target, to force it to be rebuilt.

	share 0dep hello.o
	share chkdep hello.o
	share chkdep 'set=-x' hello.o

	share lsdep hello.o

List environment settings.

	share lsenv hello.o

Create object file.

	share do hello.o
	host file hello.o

###### Executable file

Create build script.

	share lib 'tool=gcc' 'in=o' 'out=' 'file=hello'
	share catdo hello

Remove all dependencies.

	share rmdep hello

Add dependencies.

	share adddep hello 'gcc --version'
	share adddep hello hello.do
	share adddep hello hello.o

Set checksums of dependencies.

	share 0dep hello

	share lsdep hello

List environment settings.

	share lsenv hello

Create executable file and run it.

	rm hello.o
	share do 'set=-x' hello
	share do hello
	host file hello

	host obsid hello
