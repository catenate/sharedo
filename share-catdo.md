[[share build rules]]

Print commands to create files created by `.do` files.

###### Code

	share-catdo: case $# in
	share-catdo:     0)
	share-catdo:         grep -n . *.do /dev/null | grep -v 'DO NOT EDIT'
	share-catdo:         ;;
	share-catdo:     1)
	share-catdo:         cat ${1}.do | grep -v 'DO NOT EDIT'
	share-catdo:         ;;
	share-catdo: esac

###### Usage

	lit 'lit=share-catdo.md' 'file=share-catdo'

	share catdo
	share catdo hello
