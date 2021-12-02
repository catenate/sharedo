[[share build rules]]

##### Dependencies

Store dependency checksums for target `${outfile}` in the directory `${outfile}.dep/`.  For each dependency, store the path to the dependency file in a file named by the checksum of the dependency file.

This approach is also known as [[content-addressable storage]], which is part of the [[remote execution API]] used by [[pants]].
