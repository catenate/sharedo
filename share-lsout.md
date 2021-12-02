[[share build rules]]

List output files created by `.do` files.  This is a list of the targets avaliable in the current directory.

###### Code

	share-lsout: ls *.do | sed 's \.do$  '

###### Usage

	lit 'lit=share-lsout.md' 'file=share-lsout'

	share lsout
