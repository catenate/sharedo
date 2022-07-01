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

#### test

create and run a script to test output of the compiled program  #backlog 

#### cleanup

	share rmdo 'test=true'
	share rmdo

	share rmdep aloha
	share rmdep aloha.o
	share rmdep aloha.o.dep
	share rmdep helloha
	share rmdep helloha.o
	share rmdep helloha.o.dep
