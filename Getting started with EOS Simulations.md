

## Basics

always remember to `source setup.sh` first.  
Then, run `eos MACROFILENAME.mac` to run a simulation.

macro files (`.mac`) contain information about the run;  
geometry files (`.geo`) contain information about the geometry. 

macro files can be anywhere (ex, your directory) but geometry files are under

	/project/snoplus/EosSimulations/install/share/eos/ratdb/Eos/.

Yes, this is long; it might be useful to add aliases or paths to your bash script.

## Bash Basics

`cd` : move into/out of directories.  
`ls` : list what's inside directory.  
`ls -l` : list what's inside the directory, with more details.  
`pwd` : print path to current directory.  
`mkdir`, `rmdir` : make / remove directory.  
`mv`, `rm`, `cp` : move, remove, copy files. you can move/remove/copy directories and everything in them by adding `-r` (recursive).  

`mv` and `cp` always takes 2 arguments; the target file and the destination file.  
You can name your file differently as you copy/move it by simply passing it a new name, ex:

	mv filename.png images/newname.png

will move file `filename.png` into the folder `images` with a new name `newname.png`.
If you don't want to change the name, you can simply do:

	mv filename.png images/.

and it will move it into `images` with the same file name.

`.` always means current directory.  
`..` means the parent directory.  
`/` is the head directory,  
`~` is the home directory.  

i.e., no matter what directory you are in, if you do `cd /projectnb/snoplus` it will always get you to our `snoplus` folder under `projectnb` at head.  

`cd ./test_folder` or `cd ../test_folder` or `cd ~/test_folder` or `cd /test_folder` will all yield different results.

## Python scripts

`eos_electrons.mac`.  
macro script for running a simulation where an electron starts with some momentum in a given direction. Will produce a `.root` file.

`visualize.mac`.  
macro script for creating a `.wrl` file from a `.geo` file. Then, you can view the `.wrl` file using Paraview.

`visualize.py`.  
script for visualizing the number of hits per PMT in the EOS detector. Need to pass in a `.root` file, ex:

	python visualize.py ROOTFILENAME.root

Then, it'll save `.png` files under `./EventDisplay`.

## Interactive sessions

You can view files on the SCC through SCC ondemand.   
**[scc-ondemand.bu.edu](https://scc-ondemand.bu.edu/)**.   
This is the easiest way to view images and download them to your local computer.

## Geometry files

Geometry files are, again, under  

	/project/snoplus/EosSimulations/install/share/eos/ratdb/Eos/.

https://ratpac-watchman.readthedocs.io/en/latest/users_guide/geometry.html

Before viewing them on paraview, remember to create `.wrl` files using `visualize.mac`