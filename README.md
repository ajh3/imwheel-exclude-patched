# imwheel-exclude-patched

This project is a fork of imwheel-1.0.0pre12, the last official imwheel 
release from 2004. 

http://imwheel.sourceforge.net/

The only change is that the following patch has been applied, to fix 
imwheel's broken `@Exclude` command.

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

Without this patch, imwheel never successfully re-grabs a window after
you focus on an excluded window.

This patch was originally written by Milko Krachounov and posted at the link 
below. I claim no ownership over it. All I've done is apply it and create a 
repository with modern install instructions for Ubuntu, which I hope is 
helpful to others & easier to find on Google.
 
https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=260091

## To compile and install (tested on Ubuntu 18.04): 

1. `sudo apt install checkinstall libx11-dev libxtst-dev libxmu-dev`

 to install necessary dependencies for compilation.

2. `./configure `

 If any errors occur, resolve them by Googling and keep running 
./configure until they are gone.

3. `sudo checkinstall`

 We use checkinstall instead of make install in order to create a .deb
package that can be managed with your package manager. When using checkinstall,
you may want to change the version number to something like 1.1 so that
your package manager does not try replacing your patched installation with 
an unpatched version from another repository.

4. `sudo rm /etc/X11/imwheel/imwheelrc`

 By default, there is am imwheelrc file with many example settings, which
you likely do not want running on your system. Delete this file and 
add a .imwheelrc file to your home directory instead.

5. `imwheel --kill `

 to start a new instance.

That's it! You may also want to add imwheel to your system startup commands.

It no longer appears necessary on newer versions of Ubuntu to perform
the steps in the original README file related to mouse buttons 4 and 5, etc.

-Aaron Halbert
8/8/18
