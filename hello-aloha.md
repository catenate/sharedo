Compile aloha.c and hello.c to aloha.

#### aloha.o

Build script.

	share lib 'tool=gcc' 'in=c' 'out=o' 'file=aloha'

Dependencies.

	share lib 'tool=gcc' 'in=c' 'out=o.dep' 'file=aloha'
	share do aloha.o.dep

#### helloha.o

Build script.

	share lib 'tool=gcc' 'in=c' 'out=o' 'file=helloha'

Dependencies.

	share lib 'tool=gcc' 'in=c' 'out=o.dep' 'file=helloha'
	share do helloha.o.dep

#### aloha

Build script.

	share lib 'tool=gcc' 'in=o' 'out=' 'file=aloha'

Dependencies.

	share adddep aloha 'gcc --version'
	share adddep aloha aloha.o
	share adddep aloha helloha.o

#### review

	share deptree aloha

#### create object files and run executable

	nsh share do aloha
	host ./aloha

#### cleanup

	share rmdep aloha
	share rmdep helloha.o
	rm aloha
