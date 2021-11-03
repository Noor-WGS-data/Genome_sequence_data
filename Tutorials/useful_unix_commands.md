# Useful Unix Commands

This is a working document and nowhere near complete!

## Navigating and making directories

- `cd`: change what directory you’re in
`mkdir`: make directory, you can create a folder in your home directory!
- `rm`: remove; write this and then the file or folder you want to remove/delete
- `cp`: use this to copy a file from one directory and put the copy of the file in another directory. I think if you don’t specify the new location the copy just stays in the same location as the original. 
- `Contol+Z`: sometimes the $ line in front of my commands disappears and I can’t enter any commands. When I use this shortcut it terminates the failed/taking-too-long job and the (base) $ will reappear.
- `..` you can use this to navigate one folder above where you are. For example, if you're folder set up is folder > sub1 > sub2 > sub3 and you are currently in sub3 and want to get to a sub1, you can write cd `../..`

## Getting information on jobs, directories

- `pwd`: print the name/path of your current directory
- `ls`: list the contents of your current directory
- `ls -l` list the contents of your current directory plus extra informaton on each file
- `cat`: concatenate; creates files, displays contents of file, and more… 
- `head -100` shows the first n lines of the file (cause you probably don't want to look at a WGS file in it's entirety on the terminal;)).

## Shell scripts and running them
- `sbatch script.sh`: used to submit a batch script on the cluster/node you requested with #SBATCH in your .sh file. Batch processing and non-blocking (results are written to a file and you can submit other commands right away).
- `bash script.sh`: used to submit a .sh file and will run on whatever node your interactive session is logged into (i.e. head node). I used this to run test .sh scripts that are short, but if you try to run anything too long, you’ll get logged out and it will stop running (I got the error after about 3 hours: “client_loop: send disconnect: Broken pipe”
- `dos2unix`: if you’re writing your shell script on a mac and then pushing to the linux cluster, you may get this error when you try to run it ($'\r': command not found). If you run in your terminal: dos2unix path/filename, this should make sure it’s in Unix format and hopefully fix the error. 
