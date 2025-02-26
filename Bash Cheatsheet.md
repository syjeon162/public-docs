
## Bash Basics

`cd` : move into/out of directories.  
`ls` : list what's inside directory.  
`ls -l` : list what's inside the directory, with more details.  
`pwd` : print path to current directory.  

`mkdir`, `rmdir` : make / remove directory.  
`mv`, `rm`, `cp` : move, remove, copy files. you can move/remove/copy directories and everything in them by adding `-r` (recursive).  

`mv` and `cp` always takes 2 arguments; the target file and the destination file.  
You can rename your file as you copy/move it by simply passing it a new name, ex:

	mv filename.png images/newname.png

will move file `filename.png` into the folder `images` with a new name `newname.png`.  
If you don't want to change the name, you can simply do:

	mv filename.png images/.

and it will move it into the folder `images/` with the same file name.

`.` always means current directory.  
`..` means the parent directory.  
`/` is the head directory,  
`~` is the home directory.  

i.e., no matter what directory you are in, if you do `cd /projectnb/snoplus` it will always get you to our `snoplus` folder under `projectnb` at head.  

`cd ./test_folder` or `cd ../test_folder` or `cd ~/test_folder` or `cd /test_folder` will all yield different results.


### bashrc

`~/.bashrc` is a shell script that will always run when you open a terminal window or login to a cluster.  
You can set up aliases here for commands you use often. For instance:

```
alias myfolder="cd /path/to/folder"
```
will let you just type `myfolder` into the command line to execute the full command.

If you make a change to the `~/.bashrc` file, you need to `source ~/.bashrc` again for that change to be reflected.  
(or reopen the terminal window, which then will basically just do that for you)

### symbolic links

You can set up symbolic links to set up a "path" to a folder located in a different place.  
For instance, assume your file structure looks like this:  
```
/home/
├─ folderA/
│  ├─ subfolder/
│  │  ├─ file.txt
├─ folderB/
├─ folderC/
```
You can set up a symbolic link
```
ln -s /home/folderA/subfolder test
```
now you have a fake symbolic folder named `test` under `home` that is actually just `home/folderA/subfolder`:
```
/home/
├─ folderA/
│  ├─ subfolder/
│  │  ├─ file.txt
├─ folderB/
├─ folderC/
├─ test/ -> /home/folderA/subfolder/
```
ie. `file.txt` can be accessed through `/home/folderA/subfolder/file.txt` as well as `/home/test/file.txt`.  

When under `/home/test/`, you can view its "true" path by typing `pwd -P`, which will print `/home/folderA/subfolder/`, instead of `pwd`, which will yield `/home/test/`.  
Same for navigating through its original(true) file path; ie. `cd -P ..`.