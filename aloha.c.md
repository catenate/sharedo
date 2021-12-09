### Usage

Use relit to extract these files.

	host obsid relit aloha.c.md

### aloha.c

#### Header

	aloha.h: #define ALOHA "aloha"
	aloha.h: int aloha(void);

#### C source

	aloha.c: #include <stdio.h>
	aloha.c: #include "aloha.h"
	aloha.c: int aloha(void) {
	aloha.c:     printf(ALOHA "\n");
	aloha.c: }

### helloha.c

![[sharedo/hello.c.md#Header]]

#### C source

	helloha.c: #include <stdio.h>
	helloha.c: #include "aloha.h"
	helloha.c: #include "hello.h"
	helloha.c: int main(void) {
	helloha.c:     aloha();
	helloha.c:     printf(HELLO "\n");
	helloha.c: }
