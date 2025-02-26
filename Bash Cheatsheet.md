
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
