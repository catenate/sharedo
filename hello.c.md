##### Code

Header.

	hello.h: #define HELLO "hello"

For .h files, generate the lit comment as a C comment.  #testing

	lit 'lit=hello.c.md' 'file=hello.h'

C source.

	hello.c: #include <stdio.h>
	hello.c: #include "hello.h"
	hello.c: int main(void) {
	hello.c:     printf(HELLO "\n");
	hello.c: }

For .c files, generate the lit comment as a C comment.  #testing  

	lit 'lit=hello.c.md' 'file=hello.c'

##### Compile directly to an executable

	share rmdo

	share rmdep hello
	share adddep hello hello.c
	share 0dep hello

	share lib 'tool=gcc' 'in=c' 'out=' 'file=hello'
	share do hello
	host file hello

	grep -n . *.dep/* /dev/null

	host obsid hello

##### Cleanup

	share rmdo 'test=true'

	share rmdo

	share rmdep 'test=true'
