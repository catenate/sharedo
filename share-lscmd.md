[[share build rules]]

List commands to create  files created by `.do` files.  This is a list of the targets avaliable in the current directory.

###### Code

	share-lscmd: grep -n . *.do /dev/null | grep -v 'DO NOT EDIT' | grep "$1"

###### Usage

	lit 'lit=share-lscmd.md' 'file=share-lscmd'

	share lscmd
