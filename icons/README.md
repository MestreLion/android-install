Android Studio Icons
====================

Place here any icons you'd like the Android Studio installer to use. You can
delete, replace or add more icons of different sizes. The ones present here
are the official, default icons from Android Studio archive.

Filename should be in the format:

	*[-WIDTH].{png,svg}

Where `WIDTH` is, duh, the width of the image. For example, `studio-48.png`
for a 48x48 icon. Image does not have to actually be that size, it will only
be registered as such.

Also, `WIDTH` is optional: installer can auto-detect image size if you have
ImageMagick installed, which can be installed in Debian-based distros
(like Ubuntu and Linux Mint) with:

	sudo apt-get install imagemagick

SVG images without an explicit width will be installed as `scalable` size.

The image basename prefix will be ignored, as all icons will be installed
named as `android-studio`.

Icon Sources
------------

Icons were extracted from the Android Studio IDE window itself.
The window contains 2 icons, 128x128 and 32x32, as shown by `xprop`.

- `studio-128.png`, `studio-32.png`: extracted icon data from the IDE window
   using [`xdg-extract-icons`](https://github.com/MestreLion/xdg-tools), which
   reads the icon data using `xprop` and convert them to `png` images using
   ImageMagik's `convert`.

The source images of the window icon data are believed to be:

- `androidstudio.svg` (scalable, 128x128 nominal): Android Studio 4.0.1 archive,
   path `/artwork/androidstudio.svg` inside `/android-studio/lib/resources.jar`.
   Also found at `/android-studio/bin/studio.svg`.

- `androidstudio-small.svg` (scalable, 16x16 nominal): Android Studio 4.0.1 archive,
   path `/artwork/androidstudio-small.svg` inside `/android-studio/lib/resources.jar`.
