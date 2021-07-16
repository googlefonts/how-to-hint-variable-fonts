# how-to-vtt
A how-to guide to TrueType hinting variable fonts, with Visual TrueType

_by Michael Duggan_

_“The glyphs look like crisp pixel sculptures with lots of detail!”_ — Erik van Blokland

Feedback from client on a VTT Hinted Variable font

### Introduction
Font Hinting has always been thought of as a 'Black Art', some magic performed behind the scenes, hard to understand, and very difficult to do. This was indeed true, for hinting done for lower resolution screens, and older font rendering techniques, _(e.g. black and white or monochrome rendering)_. This required a lot of hinting code to control every feature in the font, to ensure consistent and clear on screen display. This made Hinting an _extremely_ time consuming, a very skilled task for specialistds, and was usually a manual process. 

The good news is, this is no longer the case. As screen resolutions and font rendering have gotton better, hinting has become a lot less complicated, with only a handful of hinting instructions needed to ensure consistent and beautiful rendering on screen. In addition, VTT's built in Autohinter, speeds up the process, leaving the focus on editing and fine tuning the hinting, to achieve the best results.

The VTT (Visual True Type) tool has been upgraded to handle all aspects of hinting for Variable fonts. The following tutorial will go into detail on all of the steps you will need, to use VTT to automatically add and then fine tune the hinting for Variable fonts, ensuring consistency and clear rendering for onscreen reading . (link to download)

### Background / Older Font Hinting.

In Digital fonts each character or glyph is described by a set of outlines. For older methods of font rendering, when the outline was scaled to a small size and rendered onto a coarse grid of pixels, each pixel whose centre lay within the outline, was set to black. This method rarely produced good results. The resulting bitmaps shapes were irregular and usually misshapen, glyph stems become uneven, spacing was not controlled, and individual glyph shapes were poor.

To help solve these problems, some form of font intelligence, or instructions, ie: Hinting was needed. These special instructions were used to help translate a typeface’s high resolution outlines, to the coarse pixel grid of a screen. To ensure even and easy to read text, every feature had be controled, including spacing, symmetry, stem consistency, individual glyph shape, proportion, & overall typographic colour. Special instructions called ‘Deltas', could be used to even further refine a glyphs shape, to turn individual pixels on or off, at a specific point size on screen. 

Achieving perfect results, however required expert hinting knowledge, a huge ammount of code, and months of painstaking detailed work. 

<img width="90%" height="90%" src="Images/oldvnewhinting.png">

**Left** Older autohinting code. In this example the hinting is intended for black and white rendering. Hinting code is added to control every aspect of the glyph for onsceeen rendering. All of the x-direction code is needed to control, spacing, proportion, symmetry, diagonal control, alignment and glyph shape. Additional Deltas would be needed to clean up the appearance of the glyph for perfectly symmetical display in black and white.

**Right** Showing new style hinting, with some simple hinting code to control x-height alignment and interpolation. All of the x-hinting code that was needed in the horizonal direction is no longer needed, and instead is rendered from the font outline, by DirectWrite subpixel rendering.

### Newer rendering and Why Hinting is still important. (title?)

Most modern Browsers and common rendering environments, such as Microsoft Office on Windows, now have full support for the latest DirectWrite (link?) rendering. VTT also has the DirectWrite font rasterizer built in. This allows you to proof your font, in the VTT Tool, while hinting, confident that the results will be replicated in the real world, in browsers and applications that use DW as their default. 

**Key Features of the DiretWrite font rendering** (link?)
 
**Subpixel Positioning**
 
When DirectWrite renders the font spacing, glyphs can begin on a subpixel boundary. Subpixel positioning, greatly improves the spacing of type on screen, especially at small sizes. The spacing accuracy that can be achieved by starting a glyphs spacing on a subpixel boundary, versus a whole pixel is significant. This more accurate spacing is critical in achieving a smooth flow and even typographic color. In addition, hinting is no longer needed to control the fonts spacing, which in the past took a lot of extra code and time to complete.
 
**Horizontal and Vertical antialiasing**
 
Subpixel rendering improves the horizontal aspect of type on screen but not the vertical. Older types of Rendering such as Cleartype in Windows GDI for example, only supported horizontal anti-aliasing. This meant that aliasing or ‘jaggies’, were still very apparent at larger sizes on screen. In order to fix this problem and give a smoother vertical effect, DirectWrite applies anti-aliasing in the y-direction, in addition to using the horizontal subpixel rendering. 
 
The addition of support for vertical anti-aliasing, helped a great deal in smoothing out the appearance of text onscreen, particularly at larger sizes. In a similar fashion, outlines are first scaled and then placed on the pixel grid of the screen. (super sampling etc……..insert antialiasing explanation). 

However, for smaller font sizes, when a fonts outline are scaled and rendered with no hinting, the vertical anti-aliasing can cause significant blur, particularly in horizontal features. **This is ‘key’ to why Hinting is still important, in particular for text reading sizes on screen with a lower resolution**  One simple set of Hinting instructions in each glyph, fits the font outline to the pixel grid, which significantly reduces the blur. Finer details such as accents benefit greatly from hinting also, allowing them to render clearly on screen, down to the smallest text sizes.  (insert more details on what hinting can do and example text, blur, clarity, avoid clashing, shape glyphs correctly for clear display, ensure complex glyphs look clear)

<img width="90%" height="90%" src="Images/EHTBLUR.png">

**Top:** Scaled un-hinted outlines, at 13ppem/10 point @96dpi, results in Blurry horizontal features. (Open Sans Variable font, default, Regular weight, ‘E’, ‘T’, ‘H’)
**Bottom:** Hinted outlines. Reduces blur, by fitting horizontals to the pixel grid. (Shown with VTT Visual Hinting)

<img width="90%" height="90%" src="Images/eotblur.png">

**Top:** Scaled un-hinted outlines results in Blurry horizontal features. (Open Sans Variable font, default, Regular weight, ‘e’, ‘o’, ‘t’)
**Bottom:** Hinted outlines, reduces blur, by fitting horizontals to the pixel grid.




