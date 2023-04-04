## Introduction
**Version control** is a system that records changes to a file or set of files over time so that you can recall specific versions later.

RCS --> CVCS --> DVCS

In a DVCS (such as **Git**, Mercurial, Bazaar or Darcs), clients don’t just check out the latest snapshot of the files; rather, they **fully mirror the repository**, including its full history. Thus, if any server dies, and these systems were collaborating via that server, any of the client repositories can be copied back up to the server to restore it. **Every clone is really a full backup of all the data.**

Git thinks of its data more like a series of snapshots of a miniature filesystem. With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored. Git thinks about its data more like a **stream of snapshots.**

Most operations in Git need only local files and resources to operate — generally no information is
needed from another computer on your network.  Because you have the entire history of the project right there on your local disk, most operations seem almost **instantaneous**. (ex 1: checking the difference between the current version of the file and the same file a month ago. ex 2: checking the entire history of the project)

Everything in Git is **checksummed** before it is stored and is then referred to by that checksum. This
means it’s impossible to change the contents of any file or directory without Git knowing about it.
This functionality is built into Git at the lowest levels and is integral to its philosophy. You can’t lose
information in transit or get file corruption without Git being able to detect it.

The mechanism that Git uses for this checksumming is called a **SHA-1 hash**. This is a 40-character
string composed of hexadecimal characters (0–9 and a–f) and calculated based on the contents of a file or directory structure in Git. A **SHA-1 hash** looks something like this:
`24b9da6552252987aa493b52f8696cd6d3b00373`

---
Pay attention now — here is the **main thing to remember about Git** if you want the rest of your
learning process to go smoothly. Git has three main states that your files can reside in: **modified,
staged, and committed:**
• **Modified** means that you have changed the file but have not committed it to your database yet.
• **Staged** means that you have marked a modified file in its current version to go into your next commit snapshot.
• **Committed** means that the data is safely stored in your local database.

This leads us to the three main sections of a Git project: **the working tree, the staging area, and the Git directory.**

The **working tree** is a single checkout of one version of the project.

The **staging area** is a file, generally contained in your Git directory, that stores information about
what will go into your next commit. Its technical name in Git parlance is the “**index**”, but the phrase
“staging area” works just as well. (sometimes also referred as **"cache"**)

The **Git directory** is where Git stores the metadata and object database for your project. This is the
most important part of Git, and it is what is copied when you clone a repository from another
computer.

**NOTE:** Project files + .git/ directory together make a repository ,also called a repo. (spelled r-ee-po)

## Git config
Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates.

Now that you have Git on your system, you’ll want to do a few things to customize your Git
environment. You should have to do these things only once on any given computer; they’ll stick
around between upgrades. You can also change them at any time by running through the
commands again.
Git comes with a tool called ==`git config`== that lets you get and set **configuration variables** (also called **keys**) that control all aspects of how Git looks and operates. These variables can be stored in three different places:
1. `[path]/etc/gitconfig file`: Contains values **applied to every user on the system** and all their
repositories. If you pass the option ==--system== to git config, it reads and writes from this file specifically. Because this is a system configuration file, you would need administrative or superuser privilege to make changes to it.
2. `~/.gitconfig or ~/.config/git/config file`: Values **specific personally to you, the user.** You can make Git read and write to this file specifically by passing the ==--global== option, and this affects all of the repositories you work with on your system.
3. config file in the Git directory (that is,` .git/config`) of whatever repository you’re currently
using: Specific to that single repository. You can force Git to read from and write to this file with
the ==--local== option, but **that is in fact the default.** Unsurprisingly, you need to be located somewhere in a Git repository for this option to work properly.

**NOTE:** Each level overrides values in the previous level, so values in .git/config trump those in `[path]/etc/gitconfig`.

You can view all of your settings and where they are coming from using:
`$git config --list --show-origin`

![[Pasted image 20230329012439.png]]
Note how the red market values are coming from the system wide config file. The yellow is coming from the file specific to the user (--global). and the green are specific to this git repository I am in. 

You may see keys more than once, because Git reads the same key from different files
(`[path]/etc/gitconfig `and `~/.gitconfig`, for example). **In this case, Git uses the last value for each unique key it sees.**

Configure your identity:
`$git config --global user.name "abhixshek"`
`$git config --global user.email abhishek.dce.2000@gmail.com`

To type your git messages, for example the commit message, git uses the default editor of your system. You can configure the editor if you wish to:
`$git config --global core.editor emacs`

You may find, if you don’t setup your editor like this, you get into a really confusing state when Git attempts to launch it. An example on a Windows system may include a prematurely terminated Git operation during a Git initiated edit.

To change your default branch name (`master`) which is given to the branch when you do `git init` , to lets say `main`:
`$git config --global init.defaultbranch main`
or
`$git config --global init.defaultBranch main`  # notice the capital B

You can also check what Git thinks a specific key’s value is by typing
`$git config <key>`
ex: 
`$git config user.name`
abhixshek
`$git config init.defaultBranch`
master

Since Git might read the same configuration variable value from more than one
file, it’s possible that you have an unexpected value for one of these values and
you don’t know why. In cases like that, you can query Git as to the origin for that
value, and it will tell you which configuration file had the final say in setting that
value:
`$git config --show-origin <key>`
ex:
`$git config --show-origin init.defaultbranch`
`file:C:/Program Files/Git/etc/gitconfig master`

If you ever need help while using Git, there are three equivalent ways to get the comprehensive
manual page (manpage) help for any of the Git commands:
`$git help <verb>`
`$git <verb> --help`
`$man git-<verb>`   # note that **man** command doesnt work on gitbash
For example, you can get the manpage help for the git config command by running this:
`$git help config`

for **add** command, `$git help add`  or  `$git add --help`

In addition, if you don’t need the full-blown manpage help, but just need a quick refresher on the
available options for a Git command, you can ask for the more concise “help” output with the ==-h==
option, as in:
`$git add -h`
![[Pasted image 20230329025643.png]]

`$git --version` # gives the version of git installed

## Git Basics
You typically obtain a Git repository in one of two ways:
1. You can take a local directory that is currently not under version control, and turn it into a Git repository, or
2. You can **clone** an existing Git repository from elsewhere.

**Initializing a Repository in an Existing Directory:**
`$cd your_proj_dir/`
`$git init`
This creates a new subdirectory named` .git` that contains all of your necessary repository files — a
Git repository skeleton.
If you want to start version-controlling existing files (as opposed to an empty directory), you should probably begin tracking those files and do an initial commit. You can accomplish that with a few `git add` commands that specify the files you want to track, followed by a `git commit`:
`$git add <file1> <file2>`
`$git add *.c`     (adds all .c files)
`$git commit -m 'initial commit'
At this point, you have a Git repository with tracked files and an initial commit.

**Cloning an Existing Repository:**
`$git clone <url>`
**NOTE** that it fetches the entire project history from the server, not just the latest working copy
ex:
`$git clone https://github.com/libgit2/libgit2`
That creates a directory named `libgit2`, initializes a` .git` directory inside it, pulls down all the data
for that repository, and checks out a working copy of the latest version.

If you want to clone the repository into a directory named something other than libgit2, you can
specify the new directory name as an additional argument:
`$git clone <url> <your_choice_dir_name>`
ex:
`$git clone https://github.com/libgit2/libgit2 mylibgit`
That command does the same thing as the previous one, but the target directory is called `mylibgit`

---
**Tracked files** are files that were in the last snapshot, as well as any newly staged files; they can be unmodified, modified, or staged. In short, tracked files are files that Git knows about.
**Untracked files** are everything else — any files in your working directory that were not in your last snapshot and are not in your staging area.

`$git status`  (to check the status of your files(untracked, unmodified, modified, staged) on the currently checked out branch. also tells where your local branch's snapshot(commit) matches the commit on the remote branch being tracked for this local branch)

To begin tracking a new file:
`$git add <filename or dir_name>`

The `git add` command takes a path name for either a file or a directory; if it’s a directory, the command adds all the files in that directory recursively.

`git add` is a multipurpose command — you use it to begin tracking new files, to stage files, and to do other things like marking merge-conflicted files as resolved. It may be helpful to think of it more as “add precisely this content to the next commit” rather than “add this file to the project”.

**NOTE:** If you modify a file after you run `git add`, you have to run `git add` again to stage the latest version of the file.

![[Pasted image 20230401022512.png]]

**NOTE:** `$git add *.c`  (adds all files ending with ".c")

>[!INFO]
`$git status -s` or `$git status --short`   (gives a short status output)
![[Pasted image 20230401022353.png]]
New files that aren’t tracked have a ?? next to them, new files that have been added to the staging area have an A, modified files have an M and so on. There are two columns to the output — the lefthand column indicates the status of the staging area and the right-hand column indicates the status of the working tree. So for example in that output, the README file is modified in the working directory but not yet staged, while the lib/simplegit.rb file is modified and staged. The Rakefile was modified, staged and then modified again, so there are changes to it that are both staged and unstaged.


### .gitignore
Often, you’ll have a class of files that you don’t want Git to automatically add or even show you as
being untracked. These are generally automatically generated files such as log files or files
produced by your build system.
Such cases can be covered in the `.gitignore` file:
````bash
$cat .gitignore
*.[oa]
*~
````
(`press ctrl+d when done`)
The first line tells Git to ignore any files ending in “.o” or “.a” — object and archive files that may be
the product of building your code.
The second line tells Git to ignore all files whose names end with a tilde (~), which is used by many text editors such as Emacs to mark temporary files.

Setting up a `.gitignore` file for your new repository before you get going is generally a good idea so you don’t accidentally commit files that you really don’t want in your Git repository.

The rules for the patterns you can put in the `.gitignore` file are as follows:
- Blank lines or lines starting with **#** are ignored.
- Standard glob patterns work, and will be applied recursively throughout the entire working tree.
- You can start patterns with a forward slash (**/**) to avoid recursivity.
- You can end patterns with a forward slash (**/**) to specify a directory.
- You can negate a pattern by starting it with an exclamation point (**!**).

**Glob patterns** are like simplified regular expressions that shells use. An asterisk `(*)` matches zero or more characters;` [abc]` matches any character inside the brackets (in this case a, b, or c); a question mark (?) matches a single character; and brackets enclosing characters separated by a hyphen `([0-9])` matches any character between them (in this case 0 through 9). You can also use two asterisks to match nested directories; `a/**/z would match a/z, a/b/z, a/b/c/z`, and so on.


>[!INFO]
>In the simple case, a repository might have a single .gitignore file in its root
directory, which applies recursively to the entire repository. However, it is also
possible to have additional .gitignore files in subdirectories. The rules in these
nested .gitignore files apply only to the files under the directory where they are
located. The Linux kernel source repository has 206 .gitignore files.

### git commit
Once you have staged the changes you want to see go in the next commit, simply run:
`$git commit`
You can see that the default commit message contains the latest output of the git status command
commented out and one empty line on top. You can remove these comments and type your commit message, or you can leave them there to help you remember what you’re committing.

type your commit message and exit using `:wq`
When you exit the editor, Git creates your commit with that commit message.

**NOTE**: an empty commit message aborts the commit.

`$git commit -v`      (the ==-v== flag shows the diff too in the commit message template so you can type your appropriate commit message while simultaneously seeing the changes you made)

Alternatively, you can type your commit message inline with the commit command by specifying it
after a ==-m== flag, like this:
`$git commit -m 'fixed bug in ml model'`

Every time you perform a commit, you’re recording a snapshot of your project that you can revert to or compare to later.

**Skipping the staging area:**
`$git commit -a -m 'added new benchmarks'`
Adding the -a option to the git commit command makes Git automatically stage every file **that is already tracked** before doing the commit, letting you skip the `git add` part.

The ==-a== flag includes all changed files. This is convenient, **but be careful; sometimes
this flag will cause you to include unwanted changes.**

Also note that, -a flag will not add untracked files to the staging area and commit them in this above command. It works only for files that are already being tracked. For untracked files, it is must to first explicitly run `git add` and then `git commit `.

---
To remove a file from Git, you have to remove it from your tracked files (more accurately, remove it
from your staging area) and then commit. The `git rm` command does that, and also removes the file from your working directory so you don’t see it as an untracked file the next time around.
`$git rm README.md`

**If you modified the file or had already added it to the staging area**, you must force the removal with the ==-f== option. This is a safety feature to prevent accidental removal of data that hasn’t yet been recorded in a snapshot and that can’t be recovered from Git.
`$git rm -f README.md`

**Another useful thing you may want to do is to keep the file in your working tree but remove it from your staging area.** In other words, you may want to keep the file on your hard drive but not have Git track it anymore. This is particularly useful if you forgot to add something to your `.gitignore` file and accidentally staged it, like a large log file or a bunch of .a compiled files. To do this, use the ==--cached== option:
`$git rm --cached README.md`     (removes README.md from the staging area, therefore it will not be part of your next commit while keeing the file on the working directory still.)
After this command `README.md` becomes an **untracked file** and you must add it again to track it if you want to. 
![[Pasted image 20230404133509.png]]

You can pass files, directories, and file-glob patterns to the git rm command. That means you can do things such as:
`$git rm logs/\*.log`
Note the backslash `(\)` in front of the `*`. This is necessary because Git does its own filename
expansion in addition to your shell’s filename expansion. This command removes all files that have
the `.log` extension in the `log/` directory.
`$git rm \*~`
This command removes all files whose names end with a `~`












![[Pasted image 20230401230012.png]]


![[Pasted image 20230401234743.png]]
![[Pasted image 20230401234821.png]]

![[Pasted image 20230401235431.png]]
![[Pasted image 20230402000829.png]]
![[Pasted image 20230402000856.png]]

