This simple, lightweight tool extracts tagged lines from a source file.   The extracted lines are each prefaced by specific text (the optional [[named parameter]] `tag`) that indicates that the line should be extracted.  The named parameter `file` names the destination file that should contain the extracted text.  The last line of the destination file will (by default, unless `nocomment=true`) contain a line reminding the reader that the file is automatically generated, so the extracted copy should not be edited.

Instead to being catenated into a file, the extracted lines may be parsed by an interpreter named by the named parameter `run`.  This named parameter may include options to the interpreter; the tag in the source file is expected to be only the name of the interpreter, which is the first word of the named parameter `run`.

Multiple output files and interpreter scripts may be extracted from the same source script.  This allows related files to appear in the same place (_eg_ literate-program file), interweaved (as long as each individual file's lines appear in executable order) and discussed together.

Line-by-line variants of a file may be extracted from the same source script.  This allows alternate versions of the file to appear in the same place, and be discussed together.  However, only one variant is in the resulting file at a time.  This may be used, for example, to temporarily debug or develop a file, while preserving the ability to generate a non-debug or stable version of the same file.

This all reasonably may be considered as elaborate commenting, which simply changes the context of a source code file from comments-in-code to code-in-comments.  The embedded lines are catenated into the destination file, or to the interpreter, in the same order that they appear in the source file.  So this method does not perform one of the most important functions of [[literate programming]]: to reorder portions of the program [in a pedagogical order](https://www.johndcook.com/blog/2016/07/06/literate-programming-presenting-code-in-human-order/), and embed named portions within other, surrounding portions.  On the other hand, it's easier to find parts of the program, since they appear in the expected order of execution, in the same order as in the extracted file, and the code is fully expanded into its final form.  An embedded named portion could instead be handled by abstracting it into another named script file, which would allow it to be tested separately, while still discussed in the same source file.

###### Usage

Extract this code to a script called `lit`.

```bash
sed -n 's,^	lit: ,,p;s,^	lit:default: ,,p' lit.md >lit
```

Test the script in the current directory.

	chmod +x lit
	host obsid which lit

This will just error, saying that the variable `lit` is not defined.

	host obsid lit

Print the file as extracted.

	host ./lit 'lit=lit.md' 'file=lit' 'run=cat'

List the available tags to extract from one [[literate program]] file.

	host ./lit 'lit=lit.md'

Cat variants without dumping them to files.

	host ./lit 'lit=lit.md' 'tag=tag' 'run=cat'
	host ./lit 'lit=lit.md' 'tag=tag' 'run=cat' 'variant=default'
	host ./lit 'lit=lit.md' 'tag=tag' 'run=cat' 'variant=variant'

Make sure there is only one file, with the current variant.

	rm tag
	host ./lit 'lit=lit.md' 'file=tag' 'run=cat'
	host ./lit 'lit=lit.md' 'file=tag' 'run=cat' 'variant=default'
	host ./lit 'lit=lit.md' 'file=tag' 'run=cat' 'variant=variant'
	cat tag

Extract a variant of this script.  We can't use lit to extract lit since a script is left open and parsed by the shell as it runs.  If it changes as it's being parsed, then the shell gets confused.

```bash
sed -n 's,^	lit: ,,p;s,^	lit:chmod: ,,p' lit.md >lit
```

###### Code

	lit: #!/usr/bin/env bash

	lit: set -eu
	lit: set -o pipefail

	lit: self=$(basename $0)

See [[argenv]].

	lit: for arg in "$@"; do
	lit: 	if echo "$arg" | grep '=' >/dev/null 2>&1; then
	lit: 		arg_name=$(echo $arg | sed 's =.*  ')
	lit: 		arg_value=$(echo $arg | sed 's ^[^=]*=  ')
	lit: 		eval ${arg_name}='${arg_value}'
	lit: 		shift
	lit: 	else
	lit: 		break
	lit: 	fi
	lit: done

	lit: test -n "$lit"

	lit: tag=${tag-}

Use `|` to separate multiple variants.

	lit: variant_in=${variant-default}
	lit: variant=$(echo ${variant-default} | sed 's | \\| g')

	lit: file=${file-}
	lit: if test -n "$file"; then
	lit: 	dir=$(dirname $file)

If we have a file and we don't have a tag, expect the file to be the tag.

	lit: 	if test -z "$tag"; then
	lit: 		tag=$file
	lit: 	fi
	lit: 	if ! test -d "$dir"; then
	lit: 		mkdir -p $dir
	lit: 	fi

Extract code lines from the literate program file, and remove the tag.

Extract lines with no variant tag, and lines with the given variant tag.  Only one variant tag is used, so each composition of multiple variants must be individually named.  This reduces the chance that the dynamic composition of several variants will result in a file that doesn't work.  #testing

	tag: always
	tag:variant: if test "$variant" = "variant"
	tag:default: if test -z "$variant", or . default variant default
	tag: also always

	lit: 	sed -n "s,^	${tag}: ,,p;s,^	${tag}:\(${variant}\): ,,p" $lit >$file

It shouldn't hurt to make the resulting file executable, even if it won't be executed directly.

	lit:default: 	chmod +x $file
	
Alternatively, only execute `chmod $chmod $file` if `$chmod` is set to something (`test -n "$chmod"`).  #testing

	lit:chmod: 	chmod=${chmod-}
	lit:chmod: 	if test -n "$chmod"; then
	lit:chmod: 	    chmod $chmod $file
	lit:chmod: 	fi

Let the reader know that this file was generated, so should not be edited, since the edits will disappear the next time the file is generated.

	lit: 	nocomment=${nocomment-false}
	lit: 	if ! $nocomment; then
	lit: 		case $file in
	lit: 		    *.c|*.h)
	lit: 		        echo "/* DO NOT EDIT THIS FILE.  Auto-generated from $variant_in $lit by $self. */" >>$file
	lit: 		        ;;
	lit: 		    *.hoon)
	lit: 		        echo ":: DO NOT EDIT THIS FILE.  Auto-generated from $variant_in $lit by $self." >>$file
	lit: 		        ;;
	lit: 		    *.json)
	lit: 		        ;;
	lit: 		    *)
	lit: 		        echo "# DO NOT EDIT THIS FILE.  Auto-generated from $variant_in $lit by $self." >>$file
	lit: 		        ;;
	lit: 		esac
	lit: 	fi

	lit: 	ext=$(echo $file | sed 's .*\.  ')
	lit: fi

	lit: run=${run-}
	lit: if test -n "$run"; then

Set the tag to the name of the interpreter, without any options after the first whitespace.

	lit: 	tag=${tag:-$(echo $run | sed 's,[	 ].*$,,')}
	lit: 	ext=$tag

If the tag is the name of a language, use the language's usual extension.

	lit: 	case $tag in
	lit: 		perl)
	lit: 			ext=pl
	lit: 			;;
	lit: 		python)
	lit: 			ext=py
	lit: 			;;
	lit: 		ruby)
	lit: 			ext=rb
	lit: 			;;
	lit: 	esac

	lit: 	sed -n "s,^	${tag}: ,,p;s,^	${tag}:\(${variant}\): ,,p" $lit >${lit}.${ext}

Run the extracted file with the given interpreter.

	lit: 	$run ${lit}.${ext} "$@"
	lit: fi

If we're not given a tag, list tags available in the given file.

	lit: if test -z "$tag"; then
	lit: 	sed -n 's,^	\([^ ]\+\): .*,\1,p' $lit | sort | uniq
	lit: fi
