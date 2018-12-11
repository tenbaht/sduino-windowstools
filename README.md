# Windows tools for sduino

This collection of files is part of the
[sdunio](https://github.com/tenbaht/sduino) environment on Windows systems.
It contains the bare minimum of command line tools needed to run the build
process either from the IDE or using a Makefile.

If you already have a working command line environment set up using tools
like mingw or cygwin you can use that and there is no need to install these
files here.

I didn't write any of the tools contained in this repository. The only
purpose of this compilation is to save you some work downloading them
separately.

This used to be a collection of [msys2](https://www.msys2.org/) tools. After
some changes in the building process and streamlining the used scripts it
was possible to replace them all by busybox and make.



## Usage

Just copy the two files `busybox.exe` and `make.exe` into a location in your
`%PATH`.



## The old MinGW collection

Is still available in the [mingw
branch](https://github.com/tenbaht/sduino-windowstools/tree/mingw) of this
repository.



## Cross-compiling the tools

*busybox.exe* is straight from the [busybox-w32](https://frippery.org/busybox/)
project, version
[FRP-2358](https://frippery.org/files/busybox/busybox-w32-FRP-2358-g25a1bcec7.exe).

*make.exe* is compiled as a 32 bit binary from the [official
sources](https://ftp.gnu.org/gnu/make/make-4.2.tar.gz) at the [GNU
Make](https://www.gnu.org/software/make/) site using mingw on a Linux host
system. Here is how to do it on a Mint 19 (Ubuntu 18.04 based) system.

Install the required packages:

	apt install mingw-w64 mingw-w64-tools

Download the source code from the [GNU ftp
server](https://ftp.gnu.org/gnu/make/), unpack, enter that directory.

	./configure --host=i686-w64-mingw32
	make INCLUDES=-I$(pwd)/glob
	strip make.exe

That's it. Ready to go.



## General problems using Windows on the command line

It works, but using the Arduino.mk makefile with Windows is slow.
There is no easy fix, the underlying problem is a fundamental one. It is not
about the compilation itself, it is the way Makefiles are written and
executed. The whole concept relies on forking subprocesses for all the shell
calls. Unfortunately, there is nothing like a fork in Windows and to work
around that is painfully slow.

Using busybox is already a huge improvement over using msys2 as now all tools
are part of the busybox binary and can be started much easier.

It might be possible to use the new Linux-Subsystem of Windows 10 instead.
(Feedback highly appreciated)
