````bash
$cd <path_from_current_dir_OR_absolute_path_to_target_dir>  (catch directory) (if you don’t specify anything it will take you to 'root dir')

$cd /home/user/catkin_ws/src/linux_course_files/move_bb8_pkg/src/

Note the /home/user/ part that appears at the beginning of the path. This is commonly known as the 'HOME' folder of the user (in this case, the user 'user'). The Home folder can be abbreviated using the character '~'. So, in this case, the above command can be substituted with the following one:
$cd ~/catkin_ws/src/linux_course_files/move_bb8_pkg/src/

$cd Desktop/    (jab puri location specify nahi kar rahe to vo ye maan ke hai ki currently jis directory me hum hein wahan se aage badhna hai)

$cd .. (move back 1 directory)
$cd ../ (same thing, move back 1 directory)
$cd ../.. (move back 2 directories)

$ls  (lists all the dir.)

$pwd (prints the current working directory)

$touch test.txt  (creates a new file named test.txt in the current dir)

$touch ~/Documents/test/mera.py  (creates a file named mera with .py extension)  Here we have given the whole location of the file.
````

To create multiple files using the `touch` command:
`$touch File1_name File2_name File3_name`

or if the files are following a certain numbering order then save yourself keystrokes and go for:
`$touch bspl{0001..0003}.c`
The above command will create 3 files named bspl001.c bspl002.c and bspl003.c

**NOTE** that .c is just the extension we have given here. Just like .txt or .py

The ugly code that follows (bspl{0001..0003}.c) is called a **brace expansion.** This is a great feature of the bash shell that allows you to create long lists of arbitrary string combinations. You can learn more about this in the Bash Hackers Wiki. In this case you will be making a long list of parameters that will be passed to touch. You can also use its long equivalent:
`$touch bspl0001.c bspl0002.c bspl0003.c`

**NOTE:**  `$cd` or `$cd ~` or `$cd /home/user` all will take you to the HOME directory.

**CASE:**
Lets say you are in `~ `dir. And you want to create folder2 in this directory like:  `~/folder1/folder2`. **Note that folder1 doesnot exist either.**
And you have to create both folders using just 1 command.
If you use
`$mkdir ~/folder1/folder2`
you will get **error**. There is only 1 way.
**Using ==-p== flag. which means create parent directory too if needed.**
`$mkdir -p ~/Documents/folder1/folder2`
**Note** that a new Documents folder will not be created since it already exists but folder1 and folder2 will be created as they do not exist.

---
`$rm test.txt (removes file named test.txt in the current dir)`

**NOTE:**
~$ - root directory = /home/ubuntu/
/$ - user directory

`$python3 ./for_loop_test.py    # ./  means from current directory onwards.`

**NOTE:**
**ctrl+alt+t** – opens the terminal window.
**ctrl+d** – closes the terminal window. **ctrl+shift+w** also does does the same thing. exit command also does the same thing.

`$mkdir abhishek   (creates folder named abhishek in the current dir)`

`$mkdir ~/catkin_ws/src/new_folder/      - created using full path.`

`$rmdir abhishek (removes the directory ONLY if it is EMPTY. that is, removes empty directory.)`

It delete a non-empty directory `dir1`, that is to delete a directory and all of its content inside it:
`$rm -r dir1`
==-r== flag means do the operation recursively. 


To remove multiple directories at once, invoke the `rm` command, followed by the names of the directories separated by space. The command below will remove each listed directory and their contents:
`$rm -r dir1 dir2 dir3`

To remove all **files** of a directory but still **keep the directory**:
`$rm dir1/*`
BUT, the above will not delete sub-folders(and their files) in dir1. So, to delete both files and folders in dir1/ :
`$rm -r dir1/*`

Use the ==-v== flag to display the steps an operation is performing. 
ex:
`$rm -rv ./*`
![[Pasted image 20230306000432.png]]

==-f== : **ignore** nonexistent files and arguments, never prompt
So, if you specify a filename that doesn't exist, it will not prompt with the message that so and so file does not exist. it will just ignore the command and do nothing. and if there are files that will be acted upon, you will not get any 2nd chance to confirm the action, that is, it will forcefully perform the operation. 
`$rm -rf __pycache__/`  (removes the `__pycache__/`  directory if it exists. removes all files and subdirectories of it recursively and doesnt prompt before removing any of them. also in case `__pycache__/`  doesnt exist in your current dir, you wont get any error, just that running the command will do nothing)

==-i== : prompt before every removal

>NOTE:
>`$man ls` (opens manual for ls command. Note that this command is specific to linux, doesn't work on gitbash)


`$cp -r source_location target_location

EX: lets say `A` is our current dir. 
```
A/
test.py
TEST_FILE
B/
C/
    file3.txt
```

`$cp TEST_FILE ./C/test_ff
`$cp test.py C/test.txt`
`$cp -r B C`
`$cp -r B C/D`

```
A/
test.py
TEST_FILE
B/
C/
    file3.txt
    test_ff
    test.txt
    B/
    D/
```

**NOTE:** that dir B was copied inside C and named D in the last command, but it is contains all the contents that B had, its just renamed to D. 
**NOTE:** that TEST_FILE is a file and yes you dont need an extension to always be defined for a file. Also, when copying/moving a file you can define some other extension for the file if you wish to change the extension as well.  (see 2nd command)

Similarly, there is a `mv` command for moving a file/dir. Again, the above examples should suffice. 

To **rename files** use `mv` command only. Just in the target destination use the new name or extension you want like below:

`$mv file1.py file_name.py`
`$mv <file_name>.<extension> ./<new_file_name>.<new_extension>`

In fact if the target location is the current dir then you don’t even need to enter `./`  just the filename.extension does the job as seen in the first command above.

---
> [!INFO]
> `cat` command - Basic file operation on a text file such as displaying or creating new files. The cat command displays file contents to a screen. Also, you can use cat command for quickly creating a file. The cat command can read and write data from standard input and output devices.
> To view a file, enter: `$cat filename`
> To create a file called “foo.txt” : `$cat >foot.txt`
> then start typing the contents of the file. To save and exit the file press ctrl+d
> To append data to a text file:` $cat >>foo.txt`
> This is **important** because if you have a file named asd.txt containing some text and you type
> `$cat > asd.txt` all of its contents will be gone and you have to retype again. i.e. using > overwrites the content of that file. Therefore to append use >> sign
> 
> To combine two or more files : `$cat score.txt names.txt > report.txt`

### Groups and their access permissions in linux:
There are 3 groups in linux -admin, group and public.
`$ls -l  (shows the permissions each group has on the files in the current dir)`
==-l== : use a long listing format
`$ls -l <filename> (if you want to see permissions for just this specific file)`

![[Pasted image 20230308123708.png]]
```
Execute – 1 (x)
Write – 2  (w)
Read – 4   (r)
```

`$chmod +x <filename> (this will give executable permission to all 3 groups i.e. admin,group and public)`

ex: lets say a file named `test` has permissions `-rwxr-xr-x` which means admin has all 3 permissions, group has execute and read permissions. And public has execute and read permission.

The first dash (-) indicates the file type where – means a regular file. (See image above)
**It is important to point out that Linux file types are not to be mistaken with file extensions.**
x corresponds to 1, w to 2, r to 4
If you want to give admin full permission (1+2+4=7) , group have rw (4+2=6), and public have only read (4), then combining the 3 numbers we get, 764, therefore do:
`$chmod 764 <filename>`
o/p: -rwxrw-r--
https://linuxconfig.org/identifying-file-types-in-linux
---

> [!INFO]
> `shift + prt screen` – to take screenshot of an area on ubuntu
> 
**shortcuts for terminal window:** 
> New Tab                                                              Shift + Ctrl + T
> 
> New Window                                                     Shift + Ctrl + N or (ctrl+alt+T)
> 
> Close Tab                                                             Shift + Ctrl + W
> 
> Close Window                                                   Shift + Ctrl + Q

### Ubuntu system management

`$ubuntu-support-status`

gives information about how long you have support for your system.

`$sudo apt update` OR `$sudo apt-get update`

==update== is used to **resynchronize** the package index files from their sources. The indexes of available packages are fetched from the location(s) specified in `/etc/apt/sources.list`. For example, when using a Debian archive, this command retrieves and scans the Packages.gz files, so that information about new and updated packages is available. An update should always be performed before an upgrade or dist-upgrade. Please be aware that the overall progress meter will be incorrect as the size of the package files cannot be known in advance.

`$cat /etc/apt/sources.list`     - to view the list of sources in sources.list file

Next, we have the option to list all packages which are scheduled for update:
`$apt list --upgradable`

`$sudo apt-get upgrade`
==upgrade== is used to install the newest versions of all packages currently installed on the system from the sources enumerated in `/etc/apt/sources.list`. Packages currently installed with new versions available are retrieved and upgraded; **under no circumstances are currently installed packages removed, or packages not already installed retrieved and installed.** New versions of currently installed packages that cannot be upgraded without changing the install status of another package will be left at their current version. An update (`$sudo apt-get update`) must be performed first so that apt-get knows that new versions of packages are available.

---
> [!INFO]
> `$python –-version`   # gives the python 2 version running in the terminal
> `$python3 –-version`   # gives the python 3 version running in the terminal
> `$python3 filename.py`  # opens the file.
> `$python`  # starts the python command line. (uses python 2)
> `$python3` # starts the python3 command line. (uses python 3)
> `>>>exit()`  # closes the python command line from the terminal.

---

1. Linux is everywhere – android,entertainment systems, DVRs, servers(google,facebook,etc), smartTVs, 498/500 supercomputers.

2. Linux is available under the GNU GPL license, which means it can be freely used on almost any product or service you’re developing, as long as the license terms are respected.Also, Linux development is community-based.

3. Linux is one of the most secure operating systems around. From devices/files to programs, access mechanisms, and secure messaging, you name it.

### what is a Linux Shell?
Well, we can say that the Shell is the tool that allows you to communicate with the Linux system in order to tell it what to do (by sending commands).

In the following demo, you are going to execute a bash script. Bash scripts are a very useful and common tool used by Linux developers. Basically, it is the tool **used for creating programs for Linux.**

![[Pasted image 20230318201429.png]]
This is known as the prompt of the Shell. It contains some basic information like the current user (in this case its user) or the current path you are on.

We can say that Linux systems are composed of 2 main elements: **folders** and **files**. The basic difference between the two is that files store data, while folders store files and other folders. The folders, often referred to as directories, are used to organize files on your computer. The folders themselves take up virtually no space on the hard drive.

The command `$ls` is a bit limited. For instance, it doesn't allow you to visualize **hidden files**.
`$ls -a`
As you can see, many new files and folders have appeared. They all have in common 1 thing, which is that **their names start with a dot** "."**.** This means that they are **hidden** (which means that they are not visible through the regular ls command, or through a default GUI).
==-a==, ==--all== : do not ignore entries starting with .

A hidden folder or hidden file is a folder or file that filesystem utilities (like `ls`) do not display by default when showing a directory listing. They are commonly used for configuration purposes, and are frequently created automatically by other utilities (which means that they are not directly created by the user).

any folder or file that starts with a dot (.), for instance `/home/user/.bashrc` (keep an eye on this file, since we will learn more about it later on in the course), will be treated as hidden.

**NOTE:**
`$<shell_command> –-help `  (will generate a menu listing all variations of that command with explanation.
alternatively, you can use the `man` command (manual):
`$man ls`

`$ls -R`  OR `$ls --recursive` (lists non-hidden files and subdirectories and their files recursively)
==-R, --recursive== : list subdirectories recursively


### "vi" visual editor
****Vi** stands for Visual. It is a text editor that is an early attempt to a visual text editor. **Vim** stands for **Vi** IMproved. It is an implementation of the **Vi** standard with many additions.*

`$vi my_file.txt`

Basically, vi has 2 different modes: Command and Insert modes:
- **Command Mode**: This mode allows you to use commands in order to interact with the file. For instance, go to a certain line of the file, delete certain lines, etc.
- **Insert Mode**: This mode allows you to insert text into the file.

By default, vi opens with the command mode activated. In order to switch to the insert mode, you will have to type the character **i**.







---
### echo
`echo` command is used in order to print something on the Shell (similar to a print function in Python).
`echo` command can be used to add/overwrite 1 or 2 lines of content to a file in the terminal itself.
`$echo 'content' > file_name.py` # this basically **redirects** the **input stream** to the file on the right. 

**NOTE**: `file_name.py` will be created if it doesn't already exist. But it will not create directories leading up to the final file.

`$echo "this is append using echo" >>./file_created_using_echo.txt `
**NOTE:** the double `>>` . this means to append the content on the left to the file on the right. 

````bash
$echo 'special characters / $ % ^ * @ & \ in this line. line added using echo' >> file_created_using_echo.txt
````

**NOTE:** if you echo using `>`  a line to a file already having some content then its content will be overwritten by the echo command run because of single `>` instead of `>>` (which means to append).

````bash
$(echo 'hi' && echo 'hello') > file.txt
$cat file.txt
hi
hello
````


