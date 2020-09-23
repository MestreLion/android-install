Android Studio Icons
====================

Place here any icons you'd like the Android Studio installer to use. You can
delete, replace or add more icons of different sizes. The ones present here
are the official, default icons from Android Studio archive.

Filename should be in the format:

	*[-WIDTH].{png,svg}

Where `WIDTH` is, the width of the image in pixels. For example, `studio-48.png`
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

The source images of the window icon data are:

- `androidstudio.svg` (scalable, 128x128 nominal): Android Studio 4.0.1 archive,
   path [`/artwork/androidstudio.svg`](sources/artwork/androidstudio.svg)
   inside `/android-studio/lib/resources.jar`.
   Also found at `/android-studio/bin/studio.svg`.

- `androidstudio-small.svg` (scalable, 16x16 nominal): Android Studio 4.0.1 archive,
   path [`/artwork/androidstudio-small.svg`](sources/artwork/androidstudio-small.svg)
   inside `/android-studio/lib/resources.jar`.

The above files are loaded as window icons in Android Studio by the following method:

- [`AndroidStudioApplicationInfo.xml`](sources/idea/AndroidStudioApplicationInfo.xml)
   inside `/android-studio/lib/resources.jar`, and possibly other `*ApplicationInfo.xml`
   files in other locations, is parsed by the IntelliJ/IDEA engine at run-time by
   [class `ApplicationInfoImpl`](sources/ApplicationInfoImpl.java). It declares
   both SVGs in:

```xml
<icon svg="/artwork/androidstudio.svg" svg-small="/artwork/androidstudio-small.svg" .../>
```

- When creating the window, [class `AppUIUtil`](sources/AppUIUtil.java) does
   `window.setIconImages(images)` with the following set of `images`:
    - On all platforms, `appInfo.getSmallApplicationSvgIconUrl()`, which turns
       out to be the `<icon svg-small>` entry in the `xml`, resized to `32` by
       function `loadApplicationIconImage()`.
    - On Unix, `appInfo.getApplicationSvgIconUrl()`, which is `<icon svg>`,
       resized to `128` by the same function above.
    - On Windows, `appInfo.getSmallApplicationSvgIconUrl()` resized to `16` by
       `loadSmallApplicationIconImage()`, which also calls `loadApplicationIconImage()`.

- To resize the images, `loadApplicationIconImage()` does not directly load the
   SVG files. Instead, calls to `IconLoader.findIcon()`, `IconUtil.toImage()`
   and `IconUtil.scale()` eventually lead to [class `SVGLoaderPrebuilt`](
   sources/SVGLoaderPrebuilt.java), which looks for pre-built, pre-scaled rastered
   versions of the SVG image in the form `file.svg-<scale>.jpix` in the path as
   the SVG file. This is meant as a cache to speed up run-time loading of SVG files.

- These cache files are generated at compile time by build script module [class
   `ImageSvgPreCompiler`](sources/ImageSvgPreCompiler.kt), which generates them
   at scales 1.0, 1.25, 1.5, 2.0 and 2.5 for all SVG files _that do **not** contain
   `xlink:href="data:`_, which happens to be the case of the large, 128x128 icon
   `androidstudio.svg`, so it does not have any pre-built files. The small 16x16
   `androidstudio-small.svg` gets the full set of these pre-built cache files.

- So, at run-time the 128x128 icon is loading from `androidstudio.svg`, while the
   small 32x32 icon data is actually taken from `androidstudio-small.svg-2.0.jpix`

- As a side note, the `.jpix` is not an actual image format, but simply the data
   from a `java.awt.BufferedImage` instance dumped to a file.

---

What about `/bin/studio.svg` and `/bin/studio.png` in the Android Studio archive?
Are they used by the Android Studio IDE? Where do they come from?

- First of all, they are _not_ used at all by the IDE. They are included in the
   archive as a mere convenience to users and third-party packagers such as
   Snapcraft or Debian to have easily-acessible icons.

- They are _not_ in this location in the source package either: they are copied
   to the archive's `/bin` by packaging scripts at **build** time.

- On all platforms, the application SVG icon, [`/artwork/androidstudio.svg`](
   sources/artwork/androidstudio.svg) is copied to `/bin/studio.svg` by
   `layoutShared()` in [`BuildTasksImpl.groovy`](sources/BuildTasksImpl.groovy).

- On Linux, [`/artwork/icon_AS_128.png`](sources/artwork/icon_AS_128.png) is
   copied to `/bin/studio.png` by function `createLinuxCustomizer()` in
   [`AndroidStudioProperties.groovy`](sources/AndroidStudioProperties.groovy).
   This file. _if_ derived from `androidstudio.svg`, did **not** use the same
   SVG rastering engine as the run-time IDE, as its image data subtly differs
   from the one extracted from the window icon.
