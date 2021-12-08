[Sharedo](https://github.com/catenate/sharedo) is a [[Bash]] variant of the [[build automation tool]] [[redo]], which constructs build systems from small shared [[shell script]]s.

###### Changes

[[git status]]: Status of the repo.

	git status
	git status --porcelain --branch
	git

[[git diff]]: Review changes.

	nsh git diff
	nsh git diff --staged

[[git add]]: Add to the index all untracked files that are not ignored.

	git add --all .

[[git commit]]: Commit the latest changes.

	git commit -a -m 'Rename lscmd to catdo.  Link all object files that are dependencies of an executable.  Take the sum of the long listing of a directory.  Adding an environment variable resets its dependency to 0.'

[[git push]]: Push latest commits to the origin git server.

	git push origin main

###### Remotes

[[git clone]]: Clone the repository.

	git clone git@github.com:catenate/sharedo.git

[[git remote]]: List remote refspecs.

	git remote -v
