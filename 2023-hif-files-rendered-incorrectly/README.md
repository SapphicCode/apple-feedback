# 2023 - Fujifilm HIF files are being rendered improperly

## TL;DR:

This is what it's supposed to look like:

![](DSCF4018.JPG)

If you're using Safari, this looks wrong:
![](DSCF4018.HIF)

This issue happens **everywhere**. Photos, Preview, Quick Look, you name it, if it uses Apple's Image API (I'm suspecting CoreImage), it will appear HDR-like and crush the shadows even if it's not an HDR.
