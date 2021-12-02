[[share build rules]]

#### Library of transformations

List transformations.

	lit 'lit=share-library.md'

##### GCC

###### gcc from c to executable

	gcc.c..do: CFLAGS=${CFLAGS-}
	gcc.c..do: gcc $CFLAGS ${1}.c -o $1

	lit 'lit=share-library.md' 'file=gcc.c..do'

###### gcc from c to o

	gcc.c.o.do: CFLAGS=${CFLAGS-}
	gcc.c.o.do: gcc $CFLAGS -c ${1}.c

	lit 'lit=share-library.md' 'file=gcc.c.o.do'

	gcc.c.o.dep.do: o=$1
	gcc.c.o.dep.do: c=$(echo $o | sed 's \.o$ .c ')
	gcc.c.o.dep.do: share-adddep $o '$CFLAGS'
	gcc.c.o.dep.do: share-adddep $o 'gcc --version'
	gcc.c.o.dep.do: share-adddep $o $c
	gcc.c.o.dep.do: share-adddep $o $o.do
	gcc.c.o.dep.do: for h in $(sed -n '/^#include "/{;s,^#include ",,;s,"$,,;p}' $c); do
	gcc.c.o.dep.do:     share-adddep $o $h
	gcc.c.o.dep.do: done

	lit 'lit=share-library.md' 'file=gcc.c.o.dep.do'

###### gcc from o to executable

	gcc.o..do: gcc ${1}.o -o $1

	lit 'lit=share-library.md' 'file=gcc.o..do'

##### Literate programs

###### lit from md to sh

	lit.md.sh.do: obsid lit lit=${1}.md run=sh

	lit 'lit=share-library.md' 'file=lit.md.sh.do'
