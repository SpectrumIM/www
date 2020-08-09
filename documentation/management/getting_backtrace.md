If Spectrum is crashing, it’s useful to get backtrace to help us to find the reason. To get a backtrace you have to have debugging symbols installed or compiled Spectrum with them.

## Installing debugging symbols

a) If you are installing from our Debian/Ubuntu repository, you can just install debugging symbols with this command:

	sudo apt-get install spectrum2-dbg libtransport-dbg

*Note:* The debug package has to be in the exact same version as the main package. So your spectrum installation might be upgraded as well when you install these packages.

b) If you build Spectrum by yourself, you have to build it in Debug mode.

	cmake . -DCMAKE_BUILD_TYPE=Debug
	make
	sudo make install

## Installing GDB

	sudo apt-get install gdb

## Getting a backtrace from a coredump

This is preferred method how to get the backtrace, because Spectrum runs without performance issues and once it crashes, it generates a coredump. 

Reproduce the crash and Spectrum will generate the coredump (file named like "core.12345" where the number is Spectrum process ID) in the working_dir (that directory is configurable in config file, default value is /var/lib/spectrum2/$jid/). Now you just have to get the backtrace from the coredump:

	cd /var/lib/spectrum/$jid/userdir
	gdb spectrum2 core.12345
	bt full

## Getting a backtrace by running Spectrum in GDB

This is harder method how to get backtrace and also running Spectrum in GDB brings performance issues. Run Spectrum in GDB:

	gdb --args spectrum2 -n config_name


where "config_name" is name of config you have in /etc/spectrum (You can also specify full path to config instead of its name).

You will see something like this:

	GNU gdb (GDB) 7.0-ubuntu
	Copyright (C) 2009 Free Software Foundation, Inc.
	License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
	This is free software: you are free to change and redistribute it.
	There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
	and "show warranty" for details.
	This GDB was configured as "i486-linux-gnu".
	For bug reporting instructions, please see:
	<http://www.gnu.org/software/gdb/bugs/>...
	Reading symbols from /home/hanzz/code/test/transport/spectrum...done.
	(gdb)

Now you have to start Spectrum with following GDB command:

	run

Since now Spectrum is running and you have to reproduce the crash or just wait for crash. Then get a backtrace with this GDB command:

	bt full

If Spectrum crashed in backend, then you need following GDB command *before* starting Spectrum:
	
	set follow-fork-mode child

And then:

	run
	bt full
	
