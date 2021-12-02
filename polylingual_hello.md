[[polyglot]]

##### Usage

Use [[lit]] to extract and run the embedded [[hello world]] scripts, from the [[literate program]] file, in an [[Acme]] window.

	host obsid lit 'lit=polylingual_hello.md' 'run=python'
	host obsid lit 'lit=polylingual_hello.md' 'run=sh'

##### Code

###### [[Python]]

	python: print("hello")

###### [[Shell]]

	sh: echo hello
