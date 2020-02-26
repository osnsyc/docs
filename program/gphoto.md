> This is a complete stab in the dark, but maybe gphoto2 is expecting the libraries to be in a different place, somewhere like /usr/lib instead of /usr/local/lib. So, you could at least test my hypothesis by using a creating-some-symbolic-links hack, and if it gphoto2 works, you could then work on finding a cleaner solution. Anyways, in case you don't know, here's what you'd type in as root to create the symbolic links:

\# ln -s /usr/local/lib/libgphoto2.so.2 /usr/lib/libgphoto2.so.2
\# ln -s /usr/local/lib/libgphoto2.so /usr/lib/libgphoto2.so

These notes describe my experiences in successfully compiling libgphoto2 
and gphoto2. 
\------------------------------------------------------------------------ 

As of February 2015 the Linux Mint (and probably Ubuntu) packaged gphoto2 is 
version 2.5.2.  In that version the command-line capture preview instruction 
does not work correctly with a Canon T3i DSLR.  The current version is 
2.5.7, in which capture preview does work correctly. 

Went to the current location of gphoto2 on my Mint Linux machine 
(/usr/bin/gphoto2) and changed the executable name to gphoto2-orig, so we 
might be able to back out of the compile, if necessary. 

Step 1: Compiling libgphoto2 
\---------------------------- 
Downloaded from gphoto2 web site http://sourceforge.net/projects/gphoto/
libgphoto2-2.5.7.tar.bz2 

Use 'tar xvf libgphoto2-2.5.7.tar.bz2' to unzip and make directory 
libgphoto2-2.5.7 

In that directory the file INSTALL contains the instructions how to 
compile and install. 

run ./configure --help 
to see the various install option. We assume (and hope) that the defaults 
will work on this system. 

run ./configure 
Output ends with: 

checking that we can compile and link with libltdl... no 
configure: error: cannot compile and link against libltdl 
libgphoto2 requires libltdl (the libtool dl* library), 
but cannot compile and link against it. 
Aborting. 

Solution: 
sudo apt-get install libltdl-dev 
That seems to have fixed that problem. 

Now when we run ./configure we get toward the end: 

 USB ports 
  libusb:             no 

 USB ports 
  libusb-1.0:           no 

Clearly we need USB support because this is a USB connected camera. 

Solution: 
sudo apt-get install libusb-1.0-0-dev 
sudo apt-get install libusb-dev 

Now when we run ./configure we get: 

 USB ports 
  libusb:             yes 

 USB ports 
  libusb-1.0:           yes 

Run 'make'. That completes without errors, apparently. 

Run 'sudo make install'. Runs without errors. 

So now libgphoto2 is updated. 

Step 2: Compiling gphoto2 
\------------------------- 
gphoto2 is available at: 
http://sourceforge.net/projects/gphoto/files/gphoto/2.5.6/

Downloaded gphoto2-2.5.6.tar. 

Extracted the archive and ran ./configure. 

checking whether popt is required... yes 
checking popt.h usability... no 
checking popt.h presence... no 
checking for popt.h... no 
checking popt.h usability... no 
checking popt.h presence... no 
checking for popt.h... no 
configure: error: 
\* Cannot autodetect popt.h 

Solution: 
sudo apt-get install libpopt-dev 

Now ./configure runs to completion without errors. 

Now run 'make' and 'sudo make install' 

These complete without errors. Run the 'which' command to find the location 
of gphoto2: 

which gphoto2 
/usr/local/bin/gphoto2 

so gphoto2 is now in /usr/local/bin and listed in PATH. 

Running gphoto2 creates an error about missing include file: 

./gphoto2: error while loading shared libraries: libgphoto2_port.so.12: 
cannot open shared object file: No such file or directory 

According to: 
http://gphoto-software.10949.n7.nabble.com/Move-to-2-5-6-failed-td14945.html:

The problem is that gphoto2 does not find the newly installed library in 
/usr/local/lib I would guess.  Can you check if /usr/local/lib is in 
/etc/ld.so.conf ?  If not add it, and rerun ldconfig. 

So... 

I edit /etc/ld.so.conf to look like this: 

/usr/local/lib 
include /etc/ld.so.conf.d/*.conf 

Now 'gphoto2 --help' works correctly. 

Now connect the camera, unmount it and run a gphoto command: 

gphoto2 --capture-image-and-download 

*** Error *** 
An error occurred in the io-library ('Could not lock the device'): Camera is 
already in use. 
ERROR: Could not capture image. 
ERROR: Could not capture. 

http://gphoto.sourceforge.net/doc/manual/FAQ.html
 This means another process on the system is using the camera already. 
Cameras supported by libgphoto2 can be accessed by file managers like 
Nautilus or Dolphin via their virtual file filesystem and their technology 
opens the cameras as soon as they are plugged in. 

One major system in use is the GNOME virtual filesystem gvfs. To detect if 
this is the case, you can run gvfs-mount -l. If this reports GPhoto device, 
your camera is likely blocked by it. In that case you can try using 
gvfs-mount -s gphoto2 to umount the virtual filesystem and replugin the 
camera. Verify with gvfs-mount -l if it is really unmounted. 

Solution: 
gvfs-mount -s gphoto2 

Now this command works correctly. 

Summary: 
\-------- 
The sequence: 'configure, make, sudo make install' basically works, but 
watch for error messages. If there is an error message, put it into Google 
and search for the solution. The error is probably a missing file that needs 
to be downloaded. 

