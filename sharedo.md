[Sharedo](https://github.com/catenate/sharedo) is a Bash variant of the build tool redo, which constructs build systems from small shared shell scripts.

###### Changes

Status of the repo.

	git status
	git status --porcelain --branch
	git

Review changes.

	nsh git diff
	nsh git diff --staged

Add to the index all untracked files that are not ignored.

	git add --all .

Commit the latest changes.

	git commit -a -m 'When lit creates .c or .h files, add a comment in C syntax.'

Push latest commits to the origin git server.

	git push origin main

###### Remotes

Clone the repository.

	git clone git@github.com:catenate/sharedo.git

List remote refspecs.

	git remote -v
