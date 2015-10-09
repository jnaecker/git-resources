# Pro Git

Here is where I'm keeping my notes as I work through [Pro Git](http://git-scm.com/book).

## Chapter 1: Getting Started

### About Version Control

* Previous version control systems (VCS) kept diffs of each file.

* Most CVS were centralized, with central server only place that stores old versions.

	* This includes Subversion.
	
	* Centralization also means single point of failure.
	
* In distributed CVS, all clients check out entire history, not just one version.

### History of Git

* In 2005, Linux community forced to develop own VCS tool.

* Goals include speed, non-linear development, fully distributed, able to handle large projects.

### Git Basics

* Git stores data as complete snapshot of filesystem.

* If a file hasn't changed, just stores a pointer back to previous version.

* Git is local; entire history of project is on your machine, so you don't need to be online to work on project.

* Every file is check-summed with SHA-1 hash, so Git will know if file has changed or is corrupted.

* Git only *adds* data, so hard to permanently mess things up once they are committed.

* In git, a file is in one of three states: **committed**, **modified**, or **staged**.

	* Committed files are stored in the *Git directory*.
	
	* Modified files are those that you have changed but not yet committed.  They live in the *working directly*, which is one version of the directory that you have checked out.
	
	*  Staged files are modified files that you have marked to be committed.  They are designated by the *staging area*, which is a files that stores data for the next commit.
	
### Installing Git

To install Git from the command line on Mac, use

	brew install git
	
if you have homebrew installed.

### First-Time Git Setup

* Use the `git config` command to set variables that control how git looks and operates.

* There are three places the configuration variables can be stored:

	*  `/etc/gitconfig` stores options for every user and all repositories on machine.  Pass `--system` option to write to this file explicitly.
	
	* `~/.gitconfig` stores options for the current user only.  Modified with the `--global` option.
	
	* `.git/config` stores options for this repository only.  Modified if no scoping option is passed.
	
* Each level overrides the one above it.

* You can edit important variables as follows:

Your identity:
	
	git config --global user.name "My Name"
	git config --global user.email "you@email.com"
	
Your default editor:
	
	git config --global core.editor vim
	
Your default diff tool:
	
	git config --global merge.tool vimdiff
	
* To check all your settings, type `git config --list`.  Or, to check a particular variable, type eg `git config user.name`.

### Getting Help

There are three equivalent ways to get help with a particular Git method:

	git help <verb>
	git <verb> --help
	man git-<verb>
	
## Chapter 2: Git Basics
	
### Getting a Git Repository

You can get a Git repo started in one of two ways:

Initialize a repo in an existing directory with `git init`:
	
	git init
	git add README
	git commit -m "Initial commit"
	
Or clone an existing repo from a server using `git clone`:

	git clone git://github.com/schacon/grit.git mygrit
	
This puts the cloned repo in directory `mygrit`.  Note that you are downloading the entire version history when you clone.

### Recording Changes to a Git Repository

Files can be either *tracked* or *untracked*.

Tracked files are those that were in the last commit.  Recall that they can be unmodified, modified, or staged.  Untracked files are any files that are not in the last commit are not added to the staging area.

To check the status of files in your directory, use `git status`.  

To track newly created files:

	git add <name>  

Note that this adds the file to the repo and stages it.  Staged files will show up under the "Changes to be committed" section when you type `git status`.  Also, note that that `git add` can take (multiple) filenames, directories, or a wildcard as an argument.
	
If you modify a file already in the repo, its `status` will be "Changes not staged for commit".  To stage, use

	git add <name>
	
as well.  (Note the multiple purposes of `add`.)  If you modify a staged file, those modifications will not be committed unless you run `git add <file>` again to stage it again.
	
You can tell Git to ignore files that you don't want to track, such as logs or other ancillary documents. To do so, put the file names in the `.gitignore` file at the top of the git directory you are tracking.  Here are the rules for `.gitignore files`:

* You can add comments with a hash (`#`).

* End patterns with a forward slash (`/`) to specify an entire directory to ignore.

* You can negate a pattern with a `!`.

* You can use glob patterns, which are simplified regex:
	
	* `*` matches zero or more characters.
	
	* `[abc]` matches any character inside the brackets.
	
	* `?` matches a single character.
	
	* `[0-9]` matches any character in the specified range. 
	
	* The pattern `**/` means any matching files in any subdirectory.
	
The command `git diff` shows you exactly what changes have been made to a file.  To see what you've changed *but not yet staged*, use

	git diff
	
To instead see what you've staged that will go into the next commit, use

	git diff --cached
	
or

	git diff --staged
	
To commit your changes, type

	git commit
	
This will open up default editor where you can type anything you want about the commit.  Any comments with `#` will be ignored in the final message.  Passing the `-v` option will include the diffs of your commit to the previous version in the editor; these will be ignored in the final message.

A faster way to commit is to include the message in quotes after the `-m` option:

	git commit -m "Fixed bug."
	
You can pass the `-a` option to `git commit` to skip the staging step and just commit any changes directly.

To remove a file, simply using the shell `rm` command will delete the file but Git will still see it as unstaged.  To remove the file from both your working directory and Git, use `git rm <filename>` and then commit this change.
	
If you modify a tracked file and then want to delete it, you'll need to pass the force option, `git rm -f <filename>`.
	
If you have staged a file and want to remove it from the staging area but leave it in your working directory, pass the `--cached` option, eg:

	git rm --cached readme.txt

Note that `git rm` can be passed filenames, directories, or glob patterns.

Finally, if you want to move or rename a file:

	git mv <from> <to>

### Viewing the Commit History

To view your commit history, use the `log` command:

	git log
	
By default, `log` displays all previous commits, with hashes, authors, date, and messages.  Most recent commits are shown first.

There are a large number of useful options to pass the `log` command:  

* `-p` shows patches, ie the diffs introduced in each commit, by line.

* `-<n>` shows the last *n* commits only.

* `--word-diff`, if appended to `git log -p`, shows diffs at the word level.

* `-U<n>` sets the number of lines of context given to diffs to *n*.
	
* `--stat` shows which files have been modified and how many lines were removed or added.

* The `--pretty` option has several parameters for making nicer-looking logs:
	
	* `--pretty=oneline` prints one line per commit.
	
	* `short`, `full`, and '`fuller` give more information.
	
	* The `format` parameter allows you to fine-tune the output in terms of content, order, length, and color.  Here is [an example](https://coderwall.com/p/euwpig/a-better-git-log]).
	
* `--graph` shows a visual representation of the branch and merge history of the repo.

* `--abrev-commit` shows only the first few characters of the hash.

* `--oneline` is a convenience option for `--pretty=oneline --abbrev-commit`.

* `--since` and `--until` control what date range of commits is shown. These can take relative arguments (eg `--since=2.weeks`) or dates `--since="2008-01-15"`.

* `--author` shows only commits written by a given author.

* `--grep="<string>"` shows only commits whose messages contain the given string.
	
* `-- <path>` shows only the commits introduced to change files in the given path.  Note that this is always the *last* option passed, and also note the double-dashes are followed by a space and then the path/filename.


