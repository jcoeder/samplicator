Instructions for installing the samplicator
===========================================

Forked to make installation instructions a little more clear.

Copyright (c) 2000-2015 Simon Leinen  <simon.leinen@gmail.com>

Install Prerequisites
--------------------------------------------------------

	yum install gcc make automake git -y

Clone the Git Repo
--------------------------------------------------------

	git clone https://github.com/sleinen/samplicator.git
	

Navigate to cloned directory
--------------------------------------------------------

`cd samplicator/`

Creating the configure script (when installing from Git)
--------------------------------------------------------

The configure script is not included in the source repository.  You
can create it using `autogen.sh`.  Note that GNU automake and GNU
autoconf are required for this.

`./autogen.sh`

Configure, Compile, and Install
-------------------------------

This distribution uses GNU configure for portability and flexibility
of installation.  You must configure the program for your system
before you can compile it using make:

`./configure`

`make`

The program can then be installed (under /usr/local/bin by default)
using "make install":

`make install`

The configure script accepts many arguments, some of which may even be
useful.  Using "./configure --prefix ..." you can specify a directory
other than /usr/local to be used as an installation destination.  Call
"./configure --help" to get a list of arguments accepted by configure.

Startup script for systemd
--------------------------

A simple `samplicator.service` systemd Service File for samplicator is
included. It works at least on CentOS 7.x, use as an example:

- modify `ExecStart` as desired for your local situation

If you ran make install with the de

`vi /etc/systemd/system/samplicator.service`

	ExecStart=/usr/local/bin/samplicate -S -c /opt/samplicator/etc/samplicator.conf -p 2055 -d 0 -f
	
- write the referred `samplicator.conf`

`vi /opt/samplicator/etc/samplicator.conf`

	172.16.1.1/255.255.255.255: 10.1.5.10/2055
	172.16.2.1/255.255.255.255: 10.1.5.10/2055
	172.16.3.1/255.255.255.255: 10.1.5.10/2055
	172.16.4.1/255.255.255.255: 10.1.5.10/2055

Then install and start the new service. On my CentOS 7.2, it looks like this:

	cp samplicator.service /etc/systemd/system/samplicator.service
	systemctl daemon-reload
	systemctl start samplicator.service
