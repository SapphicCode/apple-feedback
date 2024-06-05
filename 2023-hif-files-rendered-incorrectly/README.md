# 2023 - Fujifilm HIF files are being rendered improperly

## TL;DR:

This is what it's supposed to look like:

![a pretty black bird with shiny blue feathers sitting on a tree stump](DSCF4018.JPG)

If you're using Safari, this looks wrong:

![the same bird but now you can't see any of the pretty details in the feathers anymore](DSCF4018.HIF)

(look closely at the shadows)

This issue happens **everywhere**. Photos, Preview, Quick Look, you name it, if it uses Apple's image rendering API (I'm suspecting CoreImage), it will appear HDR-like and crush the shadows even if it's not an HDR.

## Technical details

I *suspect* it's being caused by the `nclx` color profile on the HEIF. Apple HEIFs don't use this, so they don't run into the same problems.

The above JPEG was created with ImageMagick, but I have confirmed the same behavior when a RAW is re-rendered in-camera as JPEG too. **The JPEG colors are the intended colors.** And indeed, on a modern Linux system with KDE Gwenview, they look identical.

This is pure speculation but I suspect Apple internally has re-created a similar bug that plagued libheif: https://github.com/strukturag/libheif/issues/288

## Consequences

- The image's shadows appear crushed (best case)
- The image may not render at all and instead appear as a black square
- The image behaves weirdly in Apple Photos
  - Thumbnails may fail to render
  - Thumbnails may be in the wrong orientation if the EXIF rotation tag is anything other than landscape
  - Zooming the image zooms the viewport, not the full-resolution file
  - Apple Photos might just crash or hang forever, eating up CPU in the background

## JPEG XL

The above described effects also seem to happen universally with [JPEG XL](https://jpeg.org/jpegxl/) files, so that's fun too.

Here's one of those, for completeness' sake:

![the same bird again](DSCF4018.JXL)

Props to the Safari team for being the only modern web browser to render all these images, by the way. Just, please, do it correctly.
