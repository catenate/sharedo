[[share build rules]]

Clean up files created by `.do` scripts.

###### Code

	share-rmdo: . header
	share-rmdo: log exec=true $echo rm -fr $(share-lsout)
	share-rmdo: log exec=true $echo rm -fr $(share-lsout | sed 's $ .lock ')

###### Usage

	host obsid lit 'lit=share-rmdo.md' 'file=share-rmdo'

	host obsid share-rmdo 'test=true'

	host obsid share-rmdo
