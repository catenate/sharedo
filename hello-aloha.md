Compile aloha.c and hello.c to aloha.

#### aloha.o

	share lib 'tool=gcc' 'in=c' 'out=o' 'file=aloha'
	share lib 'tool=gcc' 'in=c' 'out=o.dep' 'file=aloha'
	share do aloha.o.dep

#### helloha.o

	share lib 'tool=gcc' 'in=c' 'out=o' 'file=helloha'
	share lib 'tool=gcc' 'in=c' 'out=o.dep' 'file=helloha'
	share do helloha.o.dep

#### aloha

	share lib 'tool=gcc' 'in=o' 'out=' 'file=aloha'

	share adddep aloha 'gcc --version'
	share adddep aloha aloha.o
	share adddep aloha helloha.o

#### review

	share deptree aloha

#### create object and executable files

	nsh share do aloha
	host ./aloha

#### cleanup

	share rmdep aloha
	share rmdep helloha.o
	rm aloha
