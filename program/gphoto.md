# gphoto2 note

## Step 1: Compiling libgphoto2 

Downloaded from gphoto2 web site http://sourceforge.net/projects/gphoto/
libgphoto2-xxx.tar.bz2 

```shell
run ./configure --help 
```

to see the various install option. We assume (and hope) that the defaults 
will work on this system. 

```shell
run ./configure 

#Output ends with: 
checking that we can compile and link with libltdl... no 
configure: error: cannot compile and link against libltdl 
libgphoto2 requires libltdl (the libtool dl* library), 
but cannot compile and link against it. 
Aborting. 
```



Solution: 

```shell
sudo apt-get install libltdl-dev 
```

That seems to have fixed that problem. 

That seems to have fixed that problem. 

Now when we run ./configure we get toward the end: 

```shell
 USB ports 
  libusb:             no 

 USB ports 
  libusb-1.0:           no 
```

Clearly we need USB support because this is a USB connected camera. 

Solution: 

```shell
sudo apt-get install libusb-1.0-0-dev 
sudo apt-get install libusb-dev 
```

Now when we run ./configure we get: 

```
 USB ports 
  libusb:             yes 

 USB ports 
  libusb-1.0:           yes 
```

Run 'make'. That completes without errors, apparently. 

Run `sudo make install`. Runs without errors. 

So now libgphoto2 is updated. 

## Step 2: Compiling gphoto2 

gphoto2 is available at: 
http://sourceforge.net/projects/gphoto/files/gphoto/2.5.6/

Downloaded gphoto2-2.5.6.tar. 

Extracted the archive and ran `./configure`. 

```
checking whether popt is required... yes 
checking popt.h usability... no 
checking popt.h presence... no 
checking for popt.h... no 
checking popt.h usability... no 
checking popt.h presence... no 
checking for popt.h... no 
configure: error: 
\* Cannot autodetect popt.h 
```

Solution: 

```shell
sudo apt-get install libpopt-dev 
```

Now ./configure runs to completion without errors. 

Now run '`make`' and '`sudo make install`' 

These complete without errors. Run the 'which' command to find the location 
of gphoto2: 

```shell
which gphoto2 
#/usr/local/bin/gphoto2 
```

so gphoto2 is now in `/usr/local/bin` and listed in PATH. 

Running gphoto2 creates an error about missing include file: 

```
./gphoto2: error while loading shared libraries: libgphoto2_port.so.12: 
```


cannot open shared object file: No such file or directory 

According to: 
http://gphoto-software.10949.n7.nabble.com/Move-to-2-5-6-failed-td14945.html:

The problem is that gphoto2 does not find the newly installed library in 
`/usr/local/lib` I would guess.  Can you check if `/usr/local/lib` is in 
`/etc/ld.so.conf` ?  If not add it, and rerun ldconfig. 

So... 

I edit `/etc/ld.so.conf` to look like this: 

```
/usr/local/lib 
include /etc/ld.so.conf.d/*.conf 
```

Now 'gphoto2 --help' works correctly. 

Now connect the camera, unmount it and run a gphoto command: 

```shell
gphoto2 --capture-image-and-download 
#*** Error *** 
An error occurred in the io-library ('Could not lock the device'): Camera is 
already in use. 
ERROR: Could not capture image. 
ERROR: Could not capture. 
```

http://gphoto.sourceforge.net/doc/manual/FAQ.html
 This means another process on the system is using the camera already. 
Cameras supported by libgphoto2 can be accessed by file managers like 
Nautilus or Dolphin via their virtual file filesystem and their technology 
opens the cameras as soon as they are plugged in. 

One major system in use is the GNOME virtual filesystem gvfs. To detect if 
this is the case, you can run `gvfs-mount -l`. If this reports GPhoto device, 
your camera is likely blocked by it. In that case you can try using 
`gvfs-mount -s gphoto2` to umount the virtual filesystem and replugin the 
camera. Verify with `gvfs-mount -l` if it is really unmounted. 

Solution: 

```shell
gvfs-mount -s gphoto2 
```

Now this command works correctly. 

## TroubleShooting

### 1.creating-some-symbolic-links

> This is a complete stab in the dark, but maybe gphoto2 is expecting the libraries to be in a different place, somewhere like /usr/lib instead of /usr/local/lib. So, you could at least test my hypothesis by using a creating-some-symbolic-links hack, and if it gphoto2 works, you could then work on finding a cleaner solution. Anyways, in case you don't know, here's what you'd type in as root to create the symbolic links:

```shell
ln -s /usr/local/lib/libgphoto2.so.2 /usr/lib/libgphoto2.so.2
ln -s /usr/local/lib/libgphoto2.so /usr/lib/libgphoto2.so
```

### 2.focus control

（from [M.V.M-n.](https://mvmn.wordpress.com/)）

If you read the [documentation](http://www.gphoto.org/doc/remote/) on gphoto2 regarding the focus control (“manual” focus as they call it) it mentions that focus control only works while liveview mode is on, and that seemed to be sufficient for Canon, but it turned out that for Nikon cameras there was more to it than just liveview.

First the necessary disclaimer though: I’ve only tried the Nikon D5100 here and Canon 550D and 600D, so YMMV.

But let’s start with liveview (or preview) mode itself first – this one is controlled by “/main/actions/viewfinder” setting in both Nikon and Canon.

Essentially it’s name is “viewfinder mode”. That’s how it’s actually called by GPhotow – when you get list of camera properties via GPhoto2 on Canon it’s name is “Canon EOS Viewfinder”, and on Nikon “Nikon Viewfinder” (key is same – “/main/actions/viewfinder” – on both). So let’s not call it liveview/preview mode anymore – it’s **viewfinder** mode hereafter.

**Next thing to be aware of is that this mode is switched off when gphoto2 releases the camera. So running command-line tool “gphoto2” with parameters “–set-config /main/actions/viewfinder=1” is not the right way to do it – gphoto2 will enable viewfinder mode, then since no more commands are there it’s going to quit, and on quit it will release the camera, which will disable viewfinder mode.**

So just run gphoto2 in interactive mode (if you use gphoto2 in command line – if you use gphoto2 library and call it’s API methods you’re fine, just be aware that doing camera release – actually the method name is **[gp_camera_exit](http://gphoto.org/doc/api/gphoto2-camera_8c.html#a6f19c4ea385641fb972e779badf48ae1)** **– will disable viewfinder mode**).

Also the viewfinder mode will disable itself after some seconds of camera inactivity (you’ll hear the clicking when it does that). Maybe this is configurable somewhere in the menus, but I didn’t find out where, and in general one should just be aware of it behaving that way by default.

Ok, so we run “`gphoto2 –shell`” to start gphoto2 command-line tool in interactive mode, do “`set-config /main/actions/viewfinder=1`”, what’s next?

**Well, one more sanity check thing to do is to make sure on your lens the physical switch from auto to manual focus is set to auto – that enables your DSLR body to control the focus, but doesn’t necessarily means autofocusing. Which might be confusing for some.**

Then on Canon I could just set “`/main/actions/manualfocusdrive`” to one of those “`Near 1/2/3 or Far 1/2/3`” values and that got focus engine rotate the lens accordingly. But not on Nikon.

Experimentally I’ve found out that on Nikon I also have to set these (works for me in this specific order):

- **/main/actions/viewfinder=1** (enable viewfinder mode first, as you already know)
- **/main/capturesettings/focusmode2=MF (selection)** – don’t bother with
- **/main/capturesettings/liveviewaffocus=Single-servo AF**
- Then control focusing engine via /main/actions/manualfocusdrive (set to 1000/-1000 to see clear rotation)

Again, this is what I had found out on Nikon D5100 specifically, but I assume it’s more/less same for all or most Nikon DSLRs.

That’s it. Now go and programmatically control that focus on your Nikon :-)