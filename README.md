What is adhocify?
=================

adhocify uses inotify to watch for file system events. Once an event 
occurs it can execute a script. The path of the file and the event 
will be passed to that script.
 
Requirements
============
adhocify only runs on Linux. You need a kernel and libc with inotify 
support - pretty much all satisfy this condition.

Options
=======
See adhocify --help.

Examples:
=========
adhocify -w /tmp/ /home/user/myscript.sh
    
  * Watches for IN\_CLOSE\_WRITE events in /tmp/ and  launches script /home/user/myscript.sh

adhocify -w /tmp/ /home/user/myscript.sh {}
	
  * Same as above, but also passes the file an event occured on to that script.(argv[1]).


adhocify -w /tmp/ /bin/echo the file {} was written to
	
  * Running echo "Test" > /tmp/test will print in the shell adhocify was launched in: "the file /tmp/test was written to"

adhocify -w /tmp/ -w /var/run /home/user/myscript.sh

  * Same as above, but also watches /var/run

adhocify /home/user/myscript.sh

  * Watches for IN\_CLOSE\_WRITE events in the current directory, launches script 
	/home/user/myscript.

adhocify -m IN\_OPEN -w /tmp /home/user/myscript.sh

  * Watches for IN\_OPEN events in /tmp/, launches script /home/user/myscript.sh

adhocify -w /tmp -i *.txt /home/user/myscript.sh

  * Watches for IN\_CLOSE\_WRITE events in /tmp/ but will not pass *.txt files to the script

find . -type d | adhocify -s /home/user/myscript.sh
  
  * Starts monitoring every subdirectory of the current path for IN\_CLOSE\_WRITE events. However, inotify has 
  limits, thus it may not always work, e. g. when inotify descriptors are being used by other programs or the tree is too large. Therefore adhocify will exit
  if it cannot setup a watch for all supplied directories. See inotify(7), /proc/sys/fs/inotify/.
