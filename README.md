# Windows tools for sduino

This collection of files is part of the
[sdunio](https://github.com/tenbaht/sduino) environment on Windows systems.
It contains the bare minimum of command line tools needed to run the build
process either from the IDE or using a Makefile.

I didn't write any of the tools contained in this repository. The only
purpose of this compilation is to save you some work downloading them
separately.


## Usage

This repository is not supposed to be used separately. It is only used to
build the Windows version of the sduino-tools archive which is downloaded
automatically by the Arduino Board Manager. See the [sduino
repository](https://github.com/tenbaht/sduino) for details.

In case you still prefer a full manual install, download the repository and
copy the content of the tools directory tree into your
sduino/hardware/sduino/tools directory.

For GUI based builds that is already enough. For use of the Makefile-based
build system add the sduino/tools/win directory to your path or move the
files in there to a directory that is already in your path.



## The individual pieces

All the tools in the convinience package are 32 bit. If you need the 64 bit
versions you might have to collect them yourself.

You need `make` with some basic tools and `stm8flash`. `make` is a standard
tool included in either MinGW/Msys or cygwin. Both are fine, MinGW/Msys is
smaller. `stm8flash` is easily compiled from the [stm8flash
repository](https://github.com/vdudouyt/stm8flash).


### MinGW

[MinGW/MSys](http://www.mingw.org/wiki/MSYS) and
[cygwin](https://www.cygwin.com/) are both fine. cygwin aims to be an almost
complete POSIX environment (which is nice, but we don't need it here). MinGW
wants to be more compact and works with the native Windows API. That is good
enough for this purpose.

1. Check the
  [MinGW Installation Notes](http://www.mingw.org/wiki/Getting_Started)
2. Download
  [mingw-get-setup.exe](https://sourceforge.net/projects/mingw/files/Installer/)
  from https://sourceforge.net/projects/mingw/files/Installer/
3. Start it. You can safely deactivate the graphical option.
4. Add this at the end to your path: `;c:\mingw\bin;c:\mingw\msys\1.0\bin`
  (follow the instructions in "Environment Setting" on the [Installation
  Notes page](http://www.mingw.org/wiki/Getting_Started))
5. Open a command line and install the package msys-base by issuing this
  command: `mingw-get install msys-base`
6. Now `ls` or `make` should work.

For efficiency, the Makefile is configured to use dash instead of bash as a
shell. `egrep` is replaced by `grep -E`. The bare minimum of tools you will
need to run the Arduino.mk makefile:

	dash make
	awk cat cut expr grep head ls mkdir sed tr uname which



### stm8flash

There isn't a current precompiled windows binary for stm8flash, but it
easily compiles from the
[stm8flash repository](https://github.com/vdudouyt/stm8flash) using either
MinGW or cygwin. You will need the libusb windows binary:
http://libusb.info/ (I used the MinGW32 dll)


## General problems using Windows on the command line

It works, but using the Arduino.mk makefile with Windows is slow. **Very**
slow. Painfully slow. Compiling-the-Blink-example-takes-about-40-seconds
kind of slow. Yes, seriously. No kidding. 40 seconds. Measured on a 3GHz
machine with 4GB RAM.

There is no easy fix, the underlying problem is a fundamental one. It is not
about the compilation itself, it is the way Makefiles are written and
executed. The whole concept relies on forking subprocesses for all the shell
calls. Unfortunately, there is nothing like a fork in Windows and to work
around that is painfully slow.

It might help to replace the painfully slow parameter checking part of the
makefile (that causes the majority of forking) by a single shell script that
gets called by the makefile and delivers the results in no time. Or use
[cmake](www.cmake.org). Or to integrate it somehow into the STVD IDE. Or
whatever.

Until than the least annoying way out might be using a virtual machine
running a simple Linux system. Ubuntu Mate or a basic Debian install for
example. Virtual Box is great for this purpose and freely available. Or try
using the new Linux-Subsystem of Windows 10.
