##### aloha.c

Header.

	aloha.h: #define ALOHA "aloha"
	aloha.h: int aloha(void);

	lit 'lit=aloha.c.md' 'file=aloha.h'

C source.

	aloha.c: #include <stdio.h>
	aloha.c: #include "aloha.h"
	aloha.c: int aloha(void) {
	aloha.c:     printf(ALOHA "\n");
	aloha.c: }

	lit 'lit=aloha.c.md' 'file=aloha.c'

##### helloha.c

See hello.h in [[hello.c]].

C source.

	helloha.c: #include <stdio.h>
	helloha.c: #include "aloha.h"
	helloha.c: #include "hello.h"
	helloha.c: int main(void) {
	helloha.c:     aloha();
	helloha.c:     printf(HELLO "\n");
	helloha.c: }

For .c files, generate the lit comment as a C comment.  #testing  

	lit 'lit=aloha.c.md' 'file=helloha.c'
