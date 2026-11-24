I recently upgraded to Emacs 24.5 and my Powerline started looking [crappy](https://github.com/milkypostman/powerline/issues/54):

[![](https://www.benkuhn.net/powerline/crappy-powerline_hu57d1671762d559683606b25612d3ab3c_56068_858x98_resize_box_3.png)](https://www.benkuhn.net/powerline/crappy-powerline.png "full size")

After dealing with the crappiness for about a month, I finally got fed up and tried to figure out why.

The source of the problem appears to be that:

-   Powerline uses pixmap image “characters” to do the fancy arrow-type characters in the images.
    
-   The new version of Emacs changed the color space it rendered its characters in from RGB to sRGB.
    
-   However, it still rendered pixmap characters in sRGB.
    

(I confirmed this by using the OS X picker tool to read off the RGB color of the arrow background and the sRGB color of the normal text background; they were the same. That won’t quite work on the image above because of compression artifacts, although it’s close enough.)

I couldn’t figure out how to cause the pixmaps to render in the right color space, so the solution I decided on was to switch to using Unicode glyphs for the arrow characters—certain font variants have the right shape for unicode 25b6 (“black right-pointing triangle”) and 25c0 (“black left-pointing triangle”) that extend all the way to the edge of the character box so that they can make. Someone published [a Consolas variant](https://github.com/eugeii/consolas-powerline-vim) that included a bunch of special glyphs for Powerline.

Unfortunately, the glyphs were misaligned for me:

[![](https://www.benkuhn.net/powerline/misaligned_hua9837a5b829b4c5ffb5521b5ed063735_6804_220x78_resize_box_3.png)](https://www.benkuhn.net/powerline/misaligned.png "full size")

This is due to the Consolas font metrics being improperly set—the baseline is set way too high, which causes the characters to have no “headroom” and lots of room at the bottom. Fortunately, there’s [a way to fix this](https://wincent.com/wiki/Fixing_the_baseline_on_the_Consolas_font_on_OS_X): I downloaded the `ftxdumperfuser` tool from some weird part of Apple’s website, dumped the font metrics from the font `.ttf` to an XML file, hand-edited the XML file, and reinstalled the metrics into the ttf.

Yuck. But now everything looks only slightly misaligned!

[![](https://www.benkuhn.net/powerline/aligned_hub02fee976d607ea96cd151d919c961f1_6426_224x48_resize_box_3.png)](https://www.benkuhn.net/powerline/aligned.png "full size")