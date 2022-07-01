### Input

#### Header

	hello.in/hello.h: #define HELLO "hello"

#### C source

	hello.in/hello.c: #include <stdio.h>
	hello.in/hello.c: #include "hello.h"
	hello.in/hello.c: int main(void) {
	hello.in/hello.c:     printf(HELLO "\n");
	hello.in/hello.c: }

### Output
	hello-direxec.test.do: #/usr/bin/env bash
	hello-direxec.test.do: set -ex

	hello-direxec.test.do: outdir=$1
	hello-direxec.test.do: if ! test -d $outdir; then
	hello-direxec.test.do:     mkdir -p $outdir
	hello-direxec.test.do: fi
	hello-direxec.test.do: cd $outdir

	hello-direxec.test.do: cp -pr ../hello.in .

	hello-direxec.test.do: share-lib tool=gcc in=c out= file=hello
	hello-direxec.test.do: share-adddep hello hello.h
	hello-direxec.test.do: share-adddep hello hello.c

	hello-direxec.test.do: share-do set=-x hello
	hello-direxec.test.do: file hello
	hello-direxec.test.do: hello

### Usage

	host obsid pastout 'set=-x' hello.test.md
