# imwheel-exclude-patched

This project is a fork of `imwheel-1.0.0pre12`, [imwheel's](http://imwheel.sourceforge.net/) final official release. 

It contains only one change — a patch that repairs imwheel's broken `@Exclude` command. This lets imwheel successfully re-grab windows after focusing on an `@Exclude`d window.

```
Description: imwheel ignores FocusOut events, fix that
 imwheel's @Exclude doesn't work, because it doesn't handle FocusOut 
 events properly (explicitly ignoring them). As a result, using 
 the @Exclude command always causes imwheel to break upon entering
 an excluded window. This patch fixes this.

--- imwheel-1.0.0pre12.orig/imwheel.c
+++ imwheel-1.0.0pre12/imwheel.c
@@ -336,7 +336,7 @@ signed char getInput(Display *d, XEvent
 	else
 	{
 		e->xbutton.button=button=0;
-		e->type=None;
+		//e->type=None;
 	}
 	if(handleFocusGrab && e->type==FocusOut && (!useFifo && !grabbed))
 	{
```

Milko Krachounov [wrote this patch](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=260091). I have simply pre-applied it and updated the installation instructions.

## Compile and install (tested on Ubuntu 18.04): 

**1. `sudo apt install checkinstall libx11-dev libxtst-dev libxmu-dev`**

to install the compilation dependencies.

**2. `cd` to the `imwheel-exclude-patched-master` folder.**

**3. `./configure`**

Resolve any errors by Googling them. Repeat `./configure` until it reports no further errors.

**4. `sudo checkinstall`**

We use `checkinstall` instead of `make install` to create a portable `.deb` package.

**5. In `checkinstall`, use option `2` to rename the package to `imwheel-exclude-patched`.**

**6. Use option `3` to change the version number to `1`.**

**7. Press Enter to compile.**

**8. `sudo rm /etc/X11/imwheel/imwheelrc`**

This default `.imwheelrc` file contains several configuration examples. You likely do not want them running on your system. After removing this file, create a `.imwheelrc` file in your home directory instead.

**9. `imwheel --kill`**

to start a new instance.