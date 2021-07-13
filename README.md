# how-to-vtt
A how-to guide to TrueType hinting variable fonts, with Visual TrueType

_by Michael Duggan_

_“The glyphs look like crisp pixel sculptures with lots of detail!”_ — Erik van Blokland

Feedback from client on a VTT Hinted Variable font

### Introduction
Font Hinting has always been thought of as a 'Black Art', some magic performed behind the scenes, hard to understand, and very difficult to do. This was true, for older hinting done in the past, when lower resolution screens, and older font rendering techniques, required a lot of hinting code to control every feature in the font, to ensure consistent and clear on screen display. Hinting was an _extremely_ time consuming, skilled, and manual process. 

The good news is, this is no longer the case. As screen resolutions and font rendering have gotton better, hinting has become a lot less complicated, with only a handful of hinting instructions needed to ensure consistent and beautiful rendering on screen. In addition, VTT's built in Autohinter, will get you a long way, leaving the focus on editing and fine tuning the hinting, to achieve the best results.

The VTT (Visual True Type) tool has been upgraded to handle all aspects of hinting for Variable fonts. The following tutorial will go into detail on all of the steps you will need, to use VTT to automatically add and then fine tune the hinting for Variable fonts, ensuring consistency and clear rendering for onscreen reading . (link to download)

### Background / Older Font Hinting.

In Digital fonts each character or glyph is described by a set of outlines. For older methods of font rendering, when the outline was scaled to a small size and rendered onto a coarse grid of pixels, each pixel whose centre lay within the outline, was set to black. This method rarely produced good results. The resulting bitmaps shapes were irregular and usually misshapen, glyph stems become uneven, spacing was not controlled, and individual glyph shapes were poor.

To help solve these problems, some form of font intelligence, or instructions, ie: Hinting was needed. These special instructions were used to help translate a typeface’s high resolution outlines, to the coarse pixel grid of a screen. To ensure even and easy to read text, every feature had be controled, including spacing, symmetry, stem consistency, individual glyph shape, proportion, & overall typographic colour. Special instructions called ‘Deltas', could be used to even further refine a glyphs shape, to turn individual pixels on or off, at a specific point size on screen. 

Achieving perfect results, however required expert hinting knowledge, a huge ammount of code, and months of painstaking detailed work. 

<img width="60%" height="60%" src="Images/oldvnewhinting.png">
