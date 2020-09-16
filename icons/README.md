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

- `favicon.png` (32x32): Actual shortcut icon from
   [Android Studio website](https://developer.android.com/studio),
   extracted from its `<link rel="shortcut icon">` tag.

- `studio.png` (128x128): Android Studio 4.0.1 archive,
   path `/android-studio/bin/studio.png`

- `studio.svg` (scalable, 128x128 nominal): Android Studio 4.0.1 archive,
   path `/android-studio/bin/studio.svg`
