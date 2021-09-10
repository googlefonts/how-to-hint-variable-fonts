# how-to-vtt
A how-to guide to TrueType hinting variable fonts, with Visual TrueType

_by Michael Duggan_

_“The glyphs look like crisp pixel sculptures with lots of detail!”_ — Erik van Blokland

Feedback from client on a VTT Hinted Variable font

## Introduction
Font Hinting has always been thought of as a ‘black art’ - some magic performed behind the scenes, hard to understand, and very difficult to do. This was true when [hinting was done for low resolution screens](https://docs.microsoft.com/en-us/typography/truetype/fixing-rasterization-issues) and in the case of older font rendering techniques, such as black and white or monochrome rendering. These techniques required a lot of hinting code to ensure every feature in the font was strictly controlled and to ensure consistent and clear on-screen display. This made hinting an _extremely_ time-consuming and skilled task, for specialists only. In addition, hinting was often a manual process, involving writing and editing TrueType code by hand.

The good news is that this is no longer the case. As screen resolutions and font rendering have improved, hinting has become a lot less complicated, with only a handful of hinting instructions needed to ensure consistent and beautiful rendering on-screen. In addition, VTT’s built in Autohinter significantly speeds up the process, leaving the focus on editing and fine-tuning the hinting to achieve the best results.

**VTT (Visual TrueType)** [Install](https://aka.ms/vtt-mst)

Microsoft Visual TrueType is a software tool for viewing, editing, and adding hinting instructions to the outlines of TrueType and OpenType/TTF fonts. You use Visual TrueType after creating a font in a font editor or after converting an existing font to the TrueType format.

VTT has been upgraded to handle all aspects of hinting for variable fonts. The following tutorial will go into detail on all of the steps you will need to use VTT to automatically add, and then fine tune, the hinting for variable fonts.

## Background / Older Font Hinting

In digital fonts, each character or glyph is described by a set of outlines. For older methods of font rendering, when the outline was scaled to a small size and rendered onto a coarse grid of pixels, each pixel whose centre lay within the outline was set to black. This method never produced good results. The resulting bitmap’s shapes were irregular and usually misshapen, glyph stems became uneven, spacing was not controlled, and individual glyph shapes were poor.

To help solve these problems, some form of font intelligence or instructions  was needed. These instructions were known as 
[hints](https://docs.microsoft.com/en-us/typography/truetype/hinting), and helped to translate a typeface’s high resolution outlines to the coarse pixel grid of a screen. To ensure even and easy to read text, every feature had be controlled, including spacing, symmetry, stem consistency, individual glyph shape, proportion, and overall typographic colour. Special instructions called ‘deltas’ were often used to even further refine a glyph’s shape, turning individual pixels on or off at a specific point size on-screen. 

Achieving perfect results, however, required expert hinting knowledge, a huge amount of code, and months of painstaking detailed work. 

<img width="100%" height="100%" src="Images/oldvnewhinting.png">

**Left** _(Older autohinting code)_ Hinting intended for black and white rendering. Code is added to control every aspect of the glyph for on-screen rendering. X-direction code was required to control, spacing, proportion, symmetry, diagonal control, alignment and glyph shape. Additional deltas would be needed to clean up the appearance of the glyph for perfectly symmetrical display in black and white.

**Right** _(New style hinting)_  Greatly reduced hinting instruction set, controlling x-height alignment and interpolation. The ‘x’ hinting code that was required in the horizontal direction is no longer needed, and instead the rendering is done from the font outline using DirectWrite subpixel rendering.

## Hinting and New Rendering 

Most modern browsers and common rendering environments, such as Microsoft Office on Windows, now have full support for the latest DirectWrite rendering engine. VTT also has the DirectWrite font rasterizer built in. This allows you to proof the hinting of a variable font in the VTT Tool, without having to install the font to test the hinting results. While working in VTT, you can maintain a high level of confidence that the hinted results will be replicated in the real world, in browsers and applications that use DirectWrite as their default. Proofing is also strongly advised, on the fully hinted font, in intended target browsers and applications.

**Key Features of [DirectWrite font rendering](https://docs.microsoft.com/en-us/windows/win32/direct2d/direct2d-and-directwrite#glyph-rendering)**
 
**Subpixel Positioning**
 
When DirectWrite renders a font’s spacing, glyphs can now begin on a subpixel boundary. [Subpixel positioning](https://docs.microsoft.com/en-us/windows/win32/directwrite/introducing-directwrite#improved-text-rendering-with-cleartype) greatly improves the spacing of type on-screen, especially at small sizes. The spacing accuracy that can be achieved by starting a glyph’s spacing on a subpixel boundary is significant compared with starting on a whole pixel as in GDI. This more accurate spacing is critical in achieving a smooth flow and even typographic color to the text. 

Hinting is no longer needed to control the font’s spacing, which in the past required a lot of extra code and time to complete. This speeds up the hinting process significantly. 
 
**Horizontal and Vertical anti-aliasing**
 
Subpixel rendering improves the horizontal aspect of type on-screen but not the vertical. Older types of rendering, such as Cleartype in Windows GDI for example, only supported horizontal anti-aliasing. This meant that aliasing or ‘jaggies’ were still very apparent at larger sizes on-screen. In order to fix this problem and give a smoother vertical effect, DirectWrite applies [anti-aliasing in the y-direction](https://docs.microsoft.com/en-us/windows/win32/directwrite/introducing-directwrite#improved-text-rendering-with-cleartype) in addition to using the horizontal subpixel rendering. 
 
The addition of support for vertical anti-aliasing, helped a great deal in smoothing out the appearance of text on-screen, particularly at larger sizes. However, for smaller font sizes, when a font’s outlines are scaled and rendered with no hinting, the vertical anti-aliasing can result in significant blur, particularly for horizontal features. 

By aligning key heights and horizontal features to the pixel grid, hinting can greatly reduce this blur, in particular for text reading sizes, on lower resolution screens.

**Hinting Benefits for today’s common rendering environments**
 
**Alignment of key heights**

The main glyph heights (such as capital, x-height, figure, ascender, descender) are controlled and kept consistent on-screen for all sizes, across all variations and styles of a variable font.

**Sharp rendering of horizontal strokes and alignment zones**

A simple set of hinting instructions fits horizontal features of the font outline to the pixel grid, which significantly reduces blur. 
 
**Maintaining correct distances**

This allows all glyphs to be rendered clearly on-screen across all variations. 

Accented glyphs are hinted once to be a minimum of two pixels in height, if needed, and to be at least one pixel clear of the base glyph, for all variations. This ensures readable text across all variations, allowing for open and clear rendering on-screen, and for the detail of the font design to be maintained down to the smallest text sizes.  

Glyphs with a complex outline structure can be made to render clearly on-screen.


**Examples: Benifits of Hinting / Open Sans Variable / DirectWrite rendering**

**Alignment and blur reduction**
<img width="100%" height="100%" src="Images/EHTBLUR.png">

**Top:** Scaled un-hinted outlines, at 13ppem/10 point @96dpi, results in blurry horizontal features. _(Open Sans Variable font, default, Regular weight, ‘E’, ‘T’, ‘H’)_

**Bottom:** Hinted outlines. Reduces blur by fitting horizontals to the pixel grid. _(Shown with VTT Visual Hinting)_

<img width="100%" height="100%" src="Images/eotblur.png">

**Top:** Scaled un-hinted outlines results in blurry horizontal features. _(Open Sans Variable font, default, Regular weight, ‘e’, ‘o’, ‘t’)_

**Bottom:** Hinted outlines. Reduces blur by fitting horizontals to the pixel grid.

**Maintaining Distances**

<img width="100%" height="100%" src="Images/accentsunhintedvhinted.png">

**Top:** Accented glyphs, at 13ppem/10 point @96dpi. Accents lose details and blur together with the base glyphs, resulting in loss of legibility. _(Open Sans Variable font, default, Regular weight, and ExtraBold Variation.)_

**Bottom:** Hinted accent. Accents are hinted to preserve the correct shape and to maintain a consistent height. A minimum distance is maintained between the base glyph and the accent for all variations.


<img width="100%" height="100%" src="Images/YENBLUR.png">

**Top:** Scaled un-hinted outlines of complex glyph outlines results in blurry horizontal features.

**Bottom:** Hinted outlines reduce blur by fitting horizontals to the pixel grid, while also preserving the internal white space, allowing for clear display on-screen.

## VTT Design Space Setup

<img width="100%" height="100%" src="Images/VTTSETUP.png">

Hinting, editing, and proofing a Variable font in VTT is detailed work. It is helpful to have the workspace in VTT set up to a consistent and comfortable view. Becoming familiar with the tools, info, settings, and options will help you get the most out of VTT, and will streamline the hinting process. 

_On opening a Variation Font, you can arrange the Windows in VTT to suit your own workflow. Make any edit, and save (`ctrl + s`). This will save the VTT preferences. The next time a font file is opened in VTT you will see the same window configuration._

This example window setup _(OpenSans Variable font)_ shows the main windows (1-4) needed to view and edit the hinting, proof the results, and see the visual representation of the hinting (1 & 2), as well as the hinting code associated with each glyph. (3 & 4). Display Options (5) will not appear during the workflow when set.

**NOTE:** _Refer also to the extensive Help file in VTT for additional information on setup, proofing, Hinting basics, Hinting tools, Autohinting, Variable fonts, and more._

**(1) Main Window _(View > Main View or `ctrl + 1`)_**

Hinting is done in the Main Window _(as previously for static fonts in VTT)_ on the default instance of the Variation font. Variable fonts are a way of accessing many font instances stored in one font file, but only one set of outlines needs to be hinted: the default instance. 

With both the Main and Variation windows showing side by side, viewing and editing hints of the default instance of the font is performed in the Main window. The hinting will update live in the Variation window. Because hints in a variable font are associated with the default instance and other instances merely use interpolated CVTs (control values), you can only edit hints in the Main window.

The Variation Window can be used to proof the hinted results of any available instance in the font. When the Variation Window is highlighted, choose Display Menu > Variations Instance > Choose Variation, Light Bold, Condensed Light etc. or use keyboard shortcuts. (`ctrl + shift + up arrow / down arrow` is a quick and convenient way to toggle through the available variations in the font)

**(5) Display Options _(Menu > Display > Options or `ctrl + D`)_**

Display options settings determine how the glyph view and live text in the Main Window and Variation Window is displayed. To ensure that hinting and proofing is set correctly to show DirectWrite rendering, use the following settings shown under the ‘Rasterizer Options’ Tab:

**ClearType** 

**CT fract. AW _(subpixel positioning)_**

**CT anti-aliased _(y-smoothing)_**


These settings will be reflected in the Main View of the current glyph, the text samples in the size ramp at the bottom of the main view, and in the sample text string at the top. _(The sample text string can be customized in `Tools>options>Text Sample> Extra Text`.)_

Other settings can be configured to how best suits your own working style. 

**Useful settings tips**

`Display > Options > Design Options` >  **Show fewer points.**

Showing fewer points will hide all off curve points which are not generally needed when hinting. This also allows for a much clearer display of the glyph outline in the main window, while hinting.

`Display > Options > VTT General` > **X-direction**

Set x-direction to off, as x-hinting is not used

`Display > Options > VTT Attributes` > **Show CVT numbers.** 

Showing cvt numbers allows for easy visual proofing of the hinted glyph in the Main Window, for example to quickly determine whether the correct CVTs are used for setting heights.

**Glyph Info Bar _(Top of Main Window)_**

<img width="100%" height="100%" src="Images/Glyphinfobar.png">

While viewing any glyph in the Main Window, the glyph info bar will display the following information:

**GID number:** The Glyph ID for the currently selected glyph, in the sequential order the glyphs are stored in the font file. **Pro Tip:** The glyph order can be displayed in two ways, under `Tools > options > settings > Access glyph by Index`. When this checkbox is set, the character set (`ctrl-9`) will show the glyphs as they are ordered in the font file. It is useful to leave this as the default setting, as the hinting for every glyph in the font should be proofed and checked. If this option is unset, only glyphs with Unicode codepoints assigned will be shown. This is also a useful view, however if set as the default, some glyphs may be missed in the hinting and proofing process.

**Char:** If the glyph has an associated Unicode, this will be shown, eg: Cap H (0x48). If there is no Unicode associated with the glyph, this will be shown as Oxffff _(OpenType glyphs such as Small Caps, figure styles or ligatures, for example). Unicode is followed by the Glyph name.

**Uppercase:** Character group information for each character. The character group tells VTT which group of values to use from the Control Value Table.

**pt / ppem** Currently selected point/ppem size reflecting the selected resolution _(The resolution to display the text string and waterfall run in the main window can be changed (`Display > Resolution > choose desired resolution`). This is useful for proofing at both lower and high resolutions)_ Choose the size to view in the Main window, `Display > Size > ...`, or click on a size from the text ramp at the bottom of the Main Window. Use up and down arrows to change the point size, or `ctrl + equals`, and enter the desired size.

**grid-fitted:** Showing if hinting is turned on (grid-fitted) or off. To toggle between the two states use `ctrl + g`. 

**Pro Tip** `ctrl + g` is worn out on my keyboard. While adding hints or reviewing and editing the autohinter code, `ctrl + g` switches between hinting and no hinting. It is critical to always keep the original outline in mind, _(no hinting)_ to ensure that there is minimal distortion between the two states. 

The hinted outline should not vary too much from the high resolution design in proportion and shape, and there should be no obvious distortions.

**Pixels:** pixels on / off `ctrl + b`

**CTAA:**  Showing the current anti-aliasing settings, as chosen in `Display Options > Rasterizer settings` _(**CTAA:** ClearType anti-aliased, **GS:** Grey Scale)_

**Toolbar**

<img width="100%" height="100%" src="Images/toolbar.png">

Shown here in the main window set to the left. _(Refer to the VTT Help file for more detailed explanations on using each tool. The Toolbar can be configured to show on the top, left, right, or bottom of the Main Window using `Tools>Options>Appearance>Location`)_ 

**Note:** The ‘Move’, ‘Swap’, ‘Delete’ and ‘Insert’ commands in the Main Window UI Toolbar are disabled in VTT for Variation Fonts. Making any changes to the oultines with these commands would break a Variation font. 

Navigate / Zoomin / Zoomout 

**Pro Tip** _Using a wheel mouse for zooming in and out is a must for a smooth workflow. Zooming in to inspect details, in particular, is a common task during the hinting workflow._

**Measure**

<img width="100%" height="100%" src="Images/Measure.png">


**Left:** When Grid-fit and pixels are turned off the measuring tool will measure distances on the original outline design.

**Right** When Grid-fit and pixels are turned on the measuring tool will measure the number of pixels


**Hinting Tools**

Three main tools are needed to complete the hinting in a variable font.

**YLink Tool:** Used to constrain a distance in the y-direction between two points; for example, on a horizontal crossbar. The YLink tool can be used with reference to a value in the CVT Table. (Control Value Table)

**YShift Tool:** Used to constrain a distance in the y-direction between two points, when no CVT is used or needed _(i.e. when a distance is too small to warrant using a CVT to control the distance)_

**YInter Tool:** _(YInterpolate)_ Interpolates the position of a point between two other points.

**NOTE:** **YMove and YDelta** are not used when hinting variable fonts. These commands are only useful for static fonts. There is no way to predictably use the ‘YMove’ or ‘Delta’ command in a variable font. Deltas are special _instructions_ (used in a static TrueType font) which nudge the control points of the glyph outline at particular ppem sizes. 

**Snapshot:** Opens a new window with a copy of the Main Windows. This is an image, and only useful for comparing two outlines or comparing hinting structure. _(With the Snapshot Window open, side by side with the Main Window, you can navigate to another glyph in the Main Window to compare with the Snapshot image)_

**Changer:** Switches between the last two glyphs selected.

**(2) Variation View (`ctrl + shift + 1`)**

This window is used for viewing and proofing hinting for variation instances. The Variation Window shows you the current glyph outline and hints, in the same way as the Main Window, for the selected variable font instance. The chosen instance is displayed in the top left of the Variation Window. With both the Main and Variation windows setup side by side, you can edit hints in the Main window and see the impact on variations in the Variation window.

**Variation Cvt View** (View Menu > Variation CVT)

<img width="100%" height="100%" src="Images/varcvt.png">

The Variation CVT Window allows for adding, editing, and deleting [cvar](https://docs.microsoft.com/en-us/typography/opentype/spec/cvar) variation data in a font for axis locations in variation space, and for setting the current location in variation space in the VTT UI. 

This window can be used to adjust the numeric value of particular control values for different variation instances. _(For example, editing the value of the x-height, control value, where the outline design in a particular instance is different from the default instance)_

**(3) VTT Talk Window _(ctrl + 5)_** High Level font hinting language.

**(4) Glyph Program Window _(TrueType code) _(ctrl + 2)_** TrueType assembly code


**Proofing**

Proofing hinting can be done by viewing the sample text string at the top of the main window, and using the visual size run at the bottom of the Main Window and Variation Window. The text string at the top is useful for giving a detailed look at the how the hinted glyphs will look at any given size. To change the viewing size, use the up and down arrows or click on a size in the size ramp at the bottom. The size ramp itself is crucial for proofing the hinted results all at once for a range of sizes, and can be used not just for the default instance but for all of the variations also. _(`ctrl + shift + up arrow / down arrow` is a quick and convenient way to toggle through the available variations in the font.)_

**Waterfall _(`ctrl + 8`)_**

<img width="100%" height="100%" src="Images/waterfall.png">

The waterfall window view allows for a quick preview waterfall of the glyphs that are already in the text sample string. _(To change the text sample `Tools > Options > Extra Text`. Glyphs added here will display in the text sample at the top, in the Main and Variation Windows, as well as in the Waterfall sample)._

**Character / glyph set _(`ctrl + 9`)_**

<img width="100%" height="100%" src="Images/characterset.png">

Proofing can also be done on the entire glyph set, by choosing `View > Character / glyph set`. `ctrl + shift + up arrow / down arrow` can be used in both the Waterfall view and the Glyph set view to toggle between all of the available font variations.

> **Note:** One set of hints covers all masters in the font. If there are problems with the hints, or if you change your mind on the hinting strategy, select the Default master and edit the hints again in VTT’s main window. Remember, though, that any changes here affect all masters, so you’ll need to proof everything across all instances for the re-hinted glyphs again. 


**Choosing Glyphs**

`Edit > Go to / glyph` **(`ctrl + H`)** Enter Glyph Index or Character code

or 

Using the Character set (`ctrl + 9`). Clicking on any glyph in the character set window, will take you to that glyph in the main window. 

**Useful Keyboard Shortcuts**

* `ctrl + g`: Grid-fit _(Hinting)_ on/off. 

* `ctrl + b`: Pixels on/off. It is useful to hint with pixels turned on to visualize the exact results of the hinting code. While hinting, it is also necessary to view the outline clearly to identify control point numbers. 

* `ctrl + =`: Go to point size

* `ctrl + 9`: View Character set

* `ctrl + shift + up arrow / down arrow`:  Runs through the Variations in the Variations Windows for proofing

* `ctrl + shift + y`: Show/hide Visual Hints in the Main Window

* `Left arrow / Right arrow`: Previous / Next Glyph

**Notes on Keeping track of completed Hinting**

Keeping track of when the hinting is finished for each glyph is an important part of the workflow when hinting a variable font in VTT. _(The work to complete each glyph in the font will include editing the Autohinter output, hinting individual glyphs from scratch, and ensuring that composite glyphs are correctly positioned, as well as proofing each glyph across the variation space.)_

Hinting and proofing glyphs is rarely done in a sequential fashion. Rather, it is done in groups with similar characteristics - for example, uppercase glyphs, lowercase, figures, ligatures, etc. It is therefore important to find a scheme to keep track of when the hinting is final for each and every glyph, as VTT does not provide any ways to do this.

As an example, my own solution to this, is to use an old-school method. Set the character set (`ctrl + 9`) to order the glyphs by Glyph ID, which shows all glyphs in font. (`Tools > Options > Settings > Access Glyph by index`). Set the Character set to a comfortable size for printing or take a screen shot, and save this file as ‘Hinting Worksheet’. Use this file to markup every finalized, hinted glyph as you progress through the editing and proofing.

A color-coded system is shown here as an example of marking each glyph as having final hinted code,  edited, and proofed. In this example (Open Sans), there are four general categories for glyphs that need hinting and proofing.

1. **Unique glyphs** _(This can involve: Reviewing the Autohinter’s code, editing and fine tuning, or hinting from scratch.)_

2. **Pure composite glyphs** _(Composite of an original glyph with only offset commands in the glyph program. In this case, no code changes are necessary, only proofing. It is only the Unique reference glyph, which needs hinting)_

3. **Composites: Hinting code editing required.** Composite glyphs which use `USEMYMETRICS`, offset transformation and positioning code. At the time of writing, the code generated by the Autohinter does not work for variable fonts. The code calls a helper function (function 86) from the Font Program which uses an _outline measured distance_ as a reference for positioning accent glyphs in the y-direction. This procedure works for static fonts where there is only one measured distance, but in variable fonts, the distance between the accent and base glyph can vary. The code currently generated by the autohinter only positions the accent correctly for the default instance. 

If the measured outline distance between the base glyph and the accent is the same across the variation space, this code can be used. If the accent distance measurement varies, custom code is needed.

4. **Composites: Autohinter code output is ok**. If the autohinter's measured distance is constant, then the code is OK to use for a variable font.

<img width="100%" height="100%" src="Images/HintOS.png">



## VTT Variable font workflow process 

**Step 1.** Autohinting a Variable Font

VTT includes an Autohinter for Latin fonts. The Autohinter makes use of a lightweight hinting strategy that focuses on fitting common heights, such as x-height, cap height, ascender, descender to heights stored in the Control Value Table (CVT). This strategy anchors these key heights to the grid, thereby maintaining consistency in heights across glyphs with common heights, as well as across a font family. This method of grid-fitting also helps to reduce blur and minimizes distortion.

Follow these steps to Autohint a Latin font:

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/VTTLightLatinAutohinter.gif)

1. Start Visual True Type.
2. File > Open. Navigate to font file you would like to Autohint.
3. Select Font File and Open.
4. From the Tools menu, select Autohint > Light Latin Autohint.
5. When Autohinting is complete choose Save from File Menu

**Note** _The intent is that this Autohinter works well enough for most glyphs. Autohinting for all glyphs in the font should be carefully checked and proofed, and certain glyphs may need to be re-hinted manually, either by using the Visual Hinting tools, or by editing the VTT Talk code directly._

The Autohinter automatically generates VTTTalk (`ctrl + 5`) hinting information for each glyph, which in turn compiles down automatically to low-level TrueType instructions. (`ctrl + 2`)

VTTTalk is a high-level language designed to be easier for typographers to understand, rather than editing the more obtuse lower level true-type code. The main VTT Talk commands are used for the following types of hints

**Link** The distance between a pair of points is controlled by an entry in the CVT Table. Links always use a CVT reference. For example contolling the weight of a horizontal crossbar in the vertical direction.

**Dist** Controls the natural distance between a pair of points.

**Interpolate** Interpolates points between two parent points to find their correct position.

**Anchor** Points can be rounded to the nearest gridline, or to a grid-line specified by a CVT entry

These tables, in particular the VTTTalk tables for each glyph, is where most of the editing will be done. _Note: Composite glyphs do not contain any VTTTalk code, only low level TrueType code. Editing for composites, can only be done manually in the composite glyph program. The Visual Hinting tools are not used for Composite glyphs._

When the Light Latin Autohinter is run, VTT also creates three three global tables. 

**Control Value & cvar (cvt variations) tables)** (`ctrl + 4`)(`ctrl + shift + 4`) The CVT table is created automatically for Latin fonts, with pre-populated font measurements, saving you time, so that you can begin adding or editing the ‘Visual hinting’ in your font immediately, without the need to measure the font, and manually fill in the relevant CVT entries. The cvar table currently requires manually editing.

The two other global tables that are created are the ‘Pre-Program’ and the ‘Font Program’. 

**Pre-Program** (`ctrl + 3`) In every font there are certain conditions that will always apply. In a TrueType font these conditions are controlled through code placed in the pre-program, for example at what size Hinting wil be turned on and off globally. _Note: There is minimal editing needed in the pre-program. Changing the size for when hints are enabled and or disabled globally, is the most common edit required._

**Font Program** (`ctrl + 7`) This table stores functions that can be called from the ‘prep’ or from the glyph’s instructions. Functions are used to eliminate repetitive code from being used in multiple glyphs. A common example is to control the placement of a diacritic over a base glyph. 

Typically there is no editing done in the font program. It is useful to have a look at the comments at the beginning of the most commonly used functions, to gain a better overall understanding of what each function does.

See this documentaion for a more detailed description of the [‘Cvt’, ‘prep’ and ‘fpgm’](https://docs.microsoft.com/en-us/typography/truetype/hinting-tutorial/basic-global-tables)

VTT Also Generates a ['GASP](https://docs.microsoft.com/en-us/typography/opentype/spec/gasp) table for the font. 


## XML, Export and Import Hinting code

**Step 2.** Export, edit, import, and complile XML.

VTT allows for export and import of all hinting code to a seperate XML file. This can be used in a few ways

**Archiving:** Saving a plain text representation of the hinting code in a font.

**Outline corrections:**  Hinting is usually the last step in the production process. If there are any problems with the outlines, outline modifications will be required. VTT does allow some point manipulation for static fonts, but not for Variable outlines. Changes to the original outlines must be made in the source files, in a font editor. The font can then be regenerated. 

In summary: Hints can be exported from a TrueType font to an XML file, edits completed on the original source file outlines in a font editor, and a new TrueType file generated. Import the saved XML file, compile everything for all glyphs and resume the hinting.

**Important Note:** _This technique only works if the glyph order and point order is preserved. 

**Editing:** Export all hinting code from the font to XML, editing the XML in an external text editor, then re-importing those hints back to your original font, with modifications. 

In the following example, we will replace all ‘ResYDist’ code commands with the ‘YShift’ command. Using the YShift command in place of the ResYDist for controlling stem weights, helps to render the outlines of all Variations from Light to Black, using more natural rendering from the outline. The ResYDist command, causes stems to round to full pixels at smaller sizes. This causes distortions, particularly in lighter outlines. Using the YShift command in place of ResYDist corrects these distortions and renders the outlines more accurately.

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/exportxml.gif)

**Export all hinting data**

1. From the File menu, choose Export, then “All code to XML”.
2. Choose a location to save the file.
3. Type a filename.
4. Click Save.

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/searchreplace.gif)

**Search and replace**

1. Open XML file in WordPad or any text editor that allow for search and replace
2. Replace: Find ResYdist > replace with YShift > replace all
3. Save.

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/Compileeverything.gif)

**Import modified hinting code**

Note steps 1-3 below for exporting are not shown here, in the animation. The import takes about 30 seconds to complete for this font file.

1. Select the Import submenu of the File menu and choose “All code to XML”.
2. Select the XML file to import.
3. Click Open.
4. **Complile > Everything for all glyphs**
5. Save

All stems that previously used ResYdist to control the stem weight, will now use YShift. 

**Ensure to complete step 4 above, after editing and importing the XML file.**

## Hinting Concepts (Hinting the Capitals)

**Hinting the Control Glyphs H & O**

Let’s look at how to add hinting to the capital ‘H’, and ‘O’ using the graphical hinting tools, and take a closer look at the high-level hinting code _‘VTT Talk’_ and the lower level _‘Glyph Program’,_ TrueType code. The hinting code that is needed for these _control characters_, contains a lot of information that will translate to the rest of the glyph set.
 
Whenever the automatically generated code is not optimal, or is incorrect, it is often easier and faster to re-hint a glyph from scratch, rather than edit the autohinter code. Understanding how to use the basic graphical hinting tools, and the hinting code, makes it easier to view the visual hinting, to quickly determine if the code is correct, or if further editing or re-hinting is necessary.

Once you become familiar with some hinting concepts, tools, and code, editing the hinting or re-hinting, can be completed quickly. The automatically generated code, for the most part, will be either fully correct, or will only need some small edits.

**High level hinting strategy for hinting the uppercase H**

1. Control the top (Capital Height) and bottom (Baseline) to be consistent with other uppercase glyphs, using values in the Control Value Table as a reference. Minimise blur at the Cap Height.
 
2. Control the position of the middle horizontal bar, in relation to the baseline and Capital height. Minimise blur on the horizonal Bar.

3. Control the weight of the middle horizontal bar.
 
**Tip:** _Before beginning, ensure Auto Compile is turned on. (Tools > Options > Settings Auto-compile Main view). This will speed up the hinting, by allowing VTT to automatically generate the hinting code as you work with the Graphical Hinting tools, without the need to compile after each step in the process._

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/HintH.gif)

**Hinting the Cap H**

**Step 1: Baseline and Cap Height Control** 

Choose the YLink Tool from the Toolbar. Position the ‘blue circle’, directly over point 5, and right click. With the right mouse button held down, drag to the right to select ‘round to grid’ then release.

The following code is generated in the VTT Talk Window.

**VTT Talk**

**YAnchor (5, 8)** Moves point 5 to the control value listed in the ‘Control Program’, that corresponds to the baseline, (cvt #8) and rounds this point to a grid line. (View > Control Program:  (8: 0 /* base line */)

**YShift(5,1)** Choose the YShift Tool. Position the ‘blue circle’, directly over point 5, and drag to point 1. Shifts point 1, to a new position on the grid, relative to point 5’s new position on the grid, ensuring point 1 will also be grid-fit to the baseline.
 
**Note:** Because the capital ‘H’ is defined as ‘Uppercase’, VTT knows which group of values to use from the Control Value table, and will generate the correct values automatically.

**Cap Height Control**

Repeat Step 1, this time for point 6 at the Cap Height.

**YAnchor (6, 2)** Moves point 6 to the control value, that corresponds to the cap height, (cvt #2) and rounds this point to a grid line. (View > Control Program:  (2: 1462 /* cap height */)

**YShift(6,10)** Shifts point 10, to a new position on the grid, relative to point 6’s new position on the grid, ensuring point 10 will also be grid-fit to the cap height.

**Note:** _There are different methods that can be used in hinting. The best methods are efficient, using the least amount of code. An alternate method to using the shift command, is to YAnchor points 1 and 10, using cvt 8 and 2. The hinted result will be the same._ 

**Middle Bar Control**

Now that the Baseline and Cap Height have been established and reference the correct control values, the middle bar now needs to be interpolated to find its correct position. 

When the outline is scaled and the cap height is rounded to the grid, the outline can round up or down, depending on the size. The middle bar now needs instruction on where to find its correct position, between the baseline and the cap height.

**Step 2: YInterpolate** 

Choose the YInterpolate tool. Position the _‘blue circle’_, directly over point 5 and drag to point 6. You will see a line appear, which takes the form of a draggable _‘elastic band’._ No code is generated yet until a point is chosen to interpolate. Click anywhere on the line, and drag the curser to the point you want to interpolate, in this case point 3, and release. The following code is generated in the VTT Talk Window.

**YInterpolate(5,3,6)**

The middle bar is now positioned correcty, between the base line and cap height but one more step is needed. To maintain as much contrast as possible and to reduce blur, one side of the middle bar needs to be aligned to the pixel grid.

**Step 3: Round to grid** 

Right click on point 3, drag to the right to ‘round to grid’, and release. The YInterpolate code, will be replaced with the following code

**YIPAnchor(5,3,6)**

Moves point 3, to full pixel grid, positioned relative to point 5 and point 6, which now has a new grid-fit position. This ensures high contrast on the bottom of the horizontal crossbar on the Cap H, and helps to minimise blur. _(The Autohinter also chooses point 3 on the bottom of the horizontal bar, to interpolate, between the top and bottom of the Cap H.)_

If point 8 was chosen, the bar would have a tendency to round down and may cause the bar to look optically low. It is better visually for the bar to be high as opposed to low, the same concept as is used in the high resolution design. This same hinting strategy can then be used in a consistent way, for other glyphs with a centre bar, capital ‘E’, ‘F’, etc.

**Step 4: Control the middle horizontal bar weight**

Choose the YShift tool. Position the blue circle over point 3, and drag to point 8. The following code is generated in the VTT Talk Window.

**YShift(3,8)** Shifts point 8, to a new position, relative to point 3’s new position on the grid, maintaining the same relative distance between the point 3 and point 8 as is in the original high resolution design of the outline. The shift command does not reference a cvt value and does not move the hinted point 8 to a full pixel grid line. Shift also does not default to a one pixel minimum used by the Link command. Using the Shift command will maintain a balanced visual weight, of horizontal features in particular, across all variations.

**Step 5: Adding the Res command** 

The Res addition to the command ResAnchor, for example, stands for Rendering Environment Specific, and ensures that the appropriate rounding happens, for the various rendering environments. This saves adding additional Hinting commands if Hinting is required to work in a variety of rendering environments._The Res command calls a Function, that is designed to also allow for more subtle rendering of features such as undershoots and overshoots

_( Please refer to the longer section on Res commands for more details.)_ **(link to this)**

Switch to the VTTtalk window** (`ctlr 5`). Type Res before the YAnchor commands. Compile VTT Talk, (`ctrl r`) and save (`ctrl + s`).

The ‘Res’ _(Resolution Environment Specific)_ command is not supported when using the Graphical Hinting tools. This additional code can be added manually, in the VTTtalk window. 

The final code in the VTTtalk window will appear like this. The Res command is highlighted here for easy comparison. 
 
/* Y direction */

**Res**YAnchor(5,8)

YShift(5,1)

**Res**YAnchor(6,2)

YShift(6,10)

YIPAnchor(6,3,5)

YShift(3,8)

Smooth()

_The hinting for Cap H is now complete. The glyph can be proofed in the main window, using the text string to see shape and spacing, in the size ramp to see the hinted results at a range of sizes, and in the Variation Window, to proof for all variations in the font._

Now lets take a closer look at the lower level _‘Glyph Program’,_ TrueType code. VTTtalk compiles into TrueType instructions, which can be viewed and edited if necessary in the Glyph program. (`ctlr 2`) _Editing TrueType instructions is not commonly done, however it is useful to have an understanding of what the code is doing._

Refer to the full [TrueType Instruction](https://docs.microsoft.com/en-us/typography/opentype/spec/tt_instructions) set for details.

**NOTE:** RES Commands (Rendering Environment Specific)
_VTT supports a new set of commands, in the high-level font hinting language (“VTT Talk”). These commands replace and extend the functionality of existing commands. The new commands are added automatically by the VTT Autohinter, when Autohinting a font, and can also be added by manually editing the hinting code in the VTT Talk window and compiling. Res commands cannot be added via the graphical user interface._

_RES commands, (Rendering Environment Specific) can be used to constrain a glyph for a variety of rendering environments. The new commands map to TrueType functions, which determine the rounding granularity dynamically. To view the original TrueType code, the Res instructions can be removed and the VTTtalk compiled._ 

The following is each VTTTalk instruction followed by the corresponding TrueType Instruction, for the Capital H. _This is shown here for learning purposes. The RES commands generated by the Autohinter by default should be left in place._

**SVTCA[ Y ]** **S**et **V**ector **T**o **C**oordinate **A**xis Y. Sets the hinting direction to the Vertical or Y direction, the direction in which points will be shifted or moved. The Vector refers to the Freedom Vector, the direction in which points are moved. 

**YAnchor(5,8) > MIAP[ R ], 5, 8** /* **M**ove **I**ndirect **A**bsolute **P**oint. Moves point 5 to the value of CVT 8 and rounds point to grid. Indirect refers to the use of a CVT value, as opposed to using the actual coordinate distance to round to grid. The TrueType code for the direct method of rounding a point would be MDAP[ R ], 3 where the point is rounded to the nearest gridline, without a reference to CVT value. 

**YSHIFT(5,1) > SHP[ 1 ], 1** (Shift point by the last point) Moves point 1 maintaining the same relative distance between point 5 and point 1. 

**YAnchor(6,2) > MIAP[ R ], 6, 2** /* **M**ove **I**ndirect **A**bsolute **P**oint. Moves point 6 to the value of CVT 2 and rounds point to grid. 

**YSHIFT(6,11) > SHP[ 1 ], 1** (Shift point by the last point) Moves point 11 maintaining the same relative distance between point 6 and point 11. Note, if the point that is shifted, has the same measurement in the y-direction, the point will also be rounded to grid. Is the point to be shifted, does not have the same measurement in the y-direction, the same relative distence will be maintained as in the original outline, but will not be rounded to grid. 

SRP2[], 5 **S**et **R**eference **P**oint 2

**YIPAnchor(6,3,5) > IP[ 3 ]** Moves point 3 so that the relationship to point 5 and point 6, is the same as was in the unhinted high resolution outline. **> MDAP[ R ], 3** **M**ove **D**irect **A**bsolute **P**oint 3 Rounds point 3 to the Grid

**YSHIFT(3,8) > SHP[ 1 ], 8** (Shift point by the last point) Moves point 8 maintaining the same relative distance as in the original outline, between point 3 and point 8.

**Smooth() > IUP[ Y ] IUP[ X ]** At the end of the VTTtalk for each glyph there is a Smooth[] command. This command is then compiled into a IUP[ X ] and IUP[ Y ] TrueType instructions. These instructions move the points that have not been hinted with the points that have been hinted. The effect smoothes out the hinted outline. 

**Hinting the Cap O**

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/HintO.gif)

**High level hinting strategy for hinting the uppercase O**

Control the top _(Cap round overshoot height)_ and bottom _(Cap round undershoot height)_ to be consistent with other uppercase glyphs, using values in the Control Value Table as a reference.
 
Reduce blur at the Capital Overshoot height, and baseline undershoot height, using *inheritence, to force the undershoot and overshoot values to be equal to the baseline and square cap height until a defined size

_*Inheritence: A method of forcing one cvt to be equal to another cvt until a certain size. This is used for smaller point sizes on-screen to supress subtle features that cannot be shown when there is a limited number of pixels_
 
Control the weight of the top and bottom rounds of the O

**Step 1: Control bottom of Cap O** 

Choose the YShift Tool from the Toolbar. Position the ‘blue circle’, directly over point 5, and drag to point 22. Two lines of code are generated in the VTT Talk Window.

**YAnchor(5,9)** Moves point 5 to the control value listed in the ‘Control Program’, that corresponds to the baseline cap round undershoot value. 

**Note:** CVT 9, _(cap round undershoot)_is automatically forced to be equal to baseline cvt 8, which has a value of zero, until a higher size. CVT 3, _(cap round overshoot)_ is automatically forced to be equal to square cap height cvt 2, which has a value of 1462, until a higher size. This supresses the undershoots and overshoots, until there are enough pixels to show this subtle feature. This will already be automatically generated in the cvt table, and you will only need to change the value for appropriate point size to show the overshoots and undershoots.
 
The baseline cap round undershoot value in the cvt table looks like this.
 
**9: -22 ~ 8 @ 50 /* base line undershoot */**
 
9 is the cvt number for the cap baseline undershoot
 
-22 ~ means the relative outline measurement from baseline of 0 to bottom of the Cap 0, a measurement of -22 units.
 
8 designates the parent cvt, which is the square baseline in this case. 
 
50 means that the overshoot should kick in at 50 ppem. Replace the 50ppem by whichever ppem size you wish the overshoot to kick in.

**YShift(5,22)**

Shifts point 22, to a new position, relative to point 5’s new position on the grid, maintaining the same relative distance between the point 5 and point 22 as is in the original high-resolution design of the outline.


**Step 2: Control top of Cap O** 

Choose the YShift Tool from the Toolbar. Position the ‘blue circle’, directly over point 14, and drag to point 29. Two lines of code are generated in the VTT Talk Window.

**YAnchor(14,29)** Moves point 14 to the control value listed in the ‘Control Program’, that corresponds to the cap height overshoot.
 
3: 22 ~ 2 @ 50 /* cap height overshoot */
 
3 is the cvt number for the cap height overshoot. **Note:** 22 is an average value calculated by the autohinter. _(The actual measured difference between the square cap outline measurement of 1462, and the round cap overshoot measurement, of 1485 is 23, and the bottom round undershoot is 20. It is however better to have both of these values to be equal, to ensure the same rendering behaviour for both overshoot and undershoot at all sizes)_
 
22 ~ means the relative outline measurement from square cap height of 1462 to the top of the Cap O, an averaged value of 22 units.
 
2 designates the parent cvt, (cap height) which is the square cap height.
 
50 means that the overshoot should kick in at 50 ppem. Replace the 50ppem by whichever ppem size you wish the overshoot to kick in. The size at which the undershoot and overshoots will kick in, is usually kept to be equal.
 
**YShift(14,29)**
Shifts point 29, to a new position, relative to point 14’s new position on the grid, maintaining the same relative distance between the point 14 and point 29 as in the original high-resolution design of the outline.

**Step 3: Adding the Res command**

Switch to the VTTtalk window** (`ctlr 5`). Type Res before the YAnchor commands. Compile VTT Talk, (`ctrl r`) and save (`ctrl + s`).

The final code in the VTTtalk window will appear like this. The Res command is highlighted here for easy comparison.   /* Y direction */

**Res**YAnchor(5,9)

YShift(5,22)

**Res**YAnchor(14,3)

YShift(14,29)

Smooth()

_The hinting for Cap O is now complete. The glyph can be proofed in the main window, using the text string to see shape and spacing, in the size ramp to see the hinted results at a range of sizes, and in the Variation Window, to proof for all variations in the font._


## Editing the Capitals Hinting

<img width="100%" height="100%" src="Images/BInterpolate.png">

**Capital B Interpolation**

**Left:** Original outline design of the Capital ‘B’ in Open Sans

**Middle:** Hinted outline without interpolation

**Right:** Hinted outline with interpolation
 
**Editing the Cap B**

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/EditHintingB.gif)

As you gain more experience in looking at the code and the graphical representation of the hints, you will start to see patterns of glyphs and features that need attention.

In the Capital B, the two points 8 and 9, designed correctly in the y-direction in the original outline design, between the middle bar, become offset in the hinted version, when no interpolation is used.

The middle bar has been controlled by hinting, but points 8 and 9, now need further instruction as to how to find their correct position. As you can see in the animation this only takes a few seconds to complete.
 
Choose the YInterpolate tool. Position the ‘blue circle’, directly over point 28, at the bottom left of the middle bar, and drag to point 19. You will see a ‘line’ appear, which takes the form of a draggable ‘elastic band’. No code is generated yet until a point is chosen to interpolate. Click anywhere on the interpolaton line, and drag to the point you want to interpolate, in this case point 8 and release.

**Note:** Sometimes two points fall very closely together on the outline, so you may need to zoom in see more clearly and to ensure you pick the correct points to interpolate.

The following code is generated in the VTTtalk Window.
 
YInterpolate(19,8,28)
 
Now while zoomed in, click again anywhere on the interpolate line, drag to point 9 and release. Point 9 is now added to the interpolate code

YInterpolate(19,8,9,28)
 
That’s it for the edits on the Cap B, and the hinting is complete and ready for proofing. The points 8 and 9 now use the middle bar as a reference as to where to correctly position, for all sizes. **Note:** Interpolations by default do not round points to a grid line. In most cases this is the desired result.

While viewing the autohinted results, any kind of distortion, as described here in the B, most likely means that some additional editing and or code addition or deletion is needed. The goal for this style of hinting is always to ensure there are no obvious distortions happening the hinted outline. The closer the hinted outline is to the original design, the better the rendering will be on screen. 

<img width="100%" height="100%" src="Images/QInterpolate.png">

**Capital Q Interpolation**

**Left:** Original outline design of the Capital ‘Q’ in Open Sans

**Middle:** Hinted outline without interpolation

**Right:** Hinted outline with interpolation
 
**Editing the Cap Q**

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/EditHintingQ.gif)

Similar to the Capital B, the two points 4 and 7, are positioned correctly in the y-direction in the original outline design, between the bottom round of the Q, but become badly offset in the hinted version, when no interpolation is used.

The bottom round has been controled by hinting, but points 4 and 7, now need further instruction as to how to find their correct position in the hinted outline.
 
Using the same technique as in the B, drag the interpolation tool from the bottom of the lower round on the Q, from point 10 to point 27. Click anywhere on the interpolaton line, and drag to the point you want to interpolate, point 4, and release.

_**Note:** In this case point 4 is the most obvious point to interpolate, but on closer inspection, point 7 also needs fine control and should be included in the interpolation._

The following code is generated in the VTTtalk Window.
 
YInterpolate(27,4,7,10)
 
That’s it for the edits on the Cap Q. The hinting is complete and ready for proofing.

<img width="100%" height="100%" src="Images/KInterpolate.png">

**Capital K Interpolation**

**Left:** Original outline design of the Capital ‘K’ in Open Sans

**Middle:** Hinted outline without interpolation / middle section of the K becomes distorted and appears light

**Right:** Hinted outline with interpolation, corrects the hinted position of points 3, 8, 2 and 14
 
**Editing the Cap K**

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/EditHintingK.gif)

Editing is shown here for the default regular Weight and the light variation. The middle part of the K becomes distorted and may cause the outline to look light on-screen. It is best to proof glyphs in the main window at a low hinted size, shown here at 9 point, where it is easier to spot the distortions. 

Using the same technique as in the B, drag the interpolation tool from the baseline of the Cap K, from point 5 to point 6 at the cap height. Zoom in for easier editing. Click anywhere on the interpolaton line, and drag to the point you want to interpolate, point 3, and release. Click on the interpolation line again, and repeat for points 8, 2, and 14. 

That’s it for the edits on the Cap K. The middle points are now correctly positioned in the hinted outlines for all variations. The hinting is complete and ready for proofing. 

Editing glyphs and adding interpolations can be done in an efficient way using the graphical hinting tools. As you progress throught the glyph set and gain more experience, its becomes easier and faster to spot where and for which glyphs any additional edits are needed. Proof glyphs by turning hinting on and off, `ctrl + g` at some smaller point sizes to spot distortions.

**Editing other Capitals**

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/HintCaps.gif)

Once you become more familiar with viewing the Autohinter output, editing the hinting using the graphical hinting tools, can be done quickly. The animation above, combines the editing for the ‘A’‘M’‘R’‘N’‘S’‘V’‘W’. All of the edits are interpolations. Quickly add the interpolations and proof the glyphs in the main window and variation window.


## Hinting Concepts (Hinting the Lowercase)

**Hinting the Control Glyphs n & o**

Now, let’s look at how to add hinting to the lowercase ‘n’, and ‘o’ using the graphical hinting tools. For this example, the hinting has been deleted and the glyphs are hinted from scratch. In a typical workflow when hinting a Variable font, these glyphs will likely only need minor edits to the Autohinter output.

**High level hinting strategy for hinting the lowercase ‘o’**

Control the top _(lowercase round overshoot height)_ and bottom _(lowercase round undershoot height)_ to be consistent with other lowercase glyphs, using values in the Control Value Table as a reference.
 
Reduce blur at the lowercase overshoot height, and baseline undershoot height, using inheritence, to force the undershoot and overshoot values to be equal to the baseline and square x-height until a defined highter point size.

**High level hinting strategy for hinting the lowercase ‘n’**

1. Control the top left stem (square x-height) and top round (lowercse round overshoot) and bottom (Baseline) to be consistent with other lowercase glyphs, using values in the Control Value Table as a reference. Minimise blur at the the square and round x-height.

2. Control the weight of the top round. 
 
Using the same techniques and approach as described for the Capitals above, add the Hinting using the graphical interface hinting tools. As the glyphs are recognised as lowercase by VTT the correct cvt’s for lowercase baseline, x-height and lowercase overshoot and undershoot will be generated automatically as you add the hinting. 

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/Hintno.gif)

**Hinting the ‘o’**

The hinting approach for the lowercase ‘o’, is identical to the Capital O. Control the bottom and top rounds, and use a shift to control the weight of the top and bottom rounds. The same concept of inheritence is used for the overshoots and undershoots, the only difference is the cvt’s that are referenced in the Control Value Table, relate to the lowercase glyphs. 

This same hinting approach is used for all lowercase glyphs, with similar rounds, ‘b,c,d,e,p,q,s’, as well as the rounds of ‘f,g,h,j,m,n,r,t,u’. When all of the lowercse glyphs reference the same cvt values for these key heights, consistent alignment for baseline, x-height, ascender, descender, and overshoot and undershoot behaviour, is guaranteed onscreen for all point sizes.

**Hinting the ‘n’**

While hinting the lowercase n, there are a some additional points, that need to be controlled in addition to the baseline, square and round x-height. These points, 18 and 19 need instruction as to where to find their correct position in the hinted outline. Note here, that because of the nature of Variable font design, these points are positioned differently across the variable design space. In the default weight, the y-coordinates of the points fall on the outline in the y-direction between the baseline and the bottom the top round, while in the Condensed Bold weight, the y-coordinates of these same points are higher in the outline and fall in the y-direction between the top round. 

The best solution to ensure these points find their correct position accross the Variable design space, is to use a shift command, from point 9 to point 18. Points 18 and 19 are at the same y-coordinates, so only one point needs to be touched. _These points cannot be interpolated between the top round of the ‘n’, as they are positioned differently in different weights of the font. The y-coordinates of interpolated points must fall between the two parent points, referenced in the Interpolation._

**Edit the Hinting for the lowercase ‘i’**

The lowercase ‘i’ is a great example of why it is impotant to have a hinting stragety in mind, before adding the hints. In the high resolution design, the dot on the ‘i’, is a seperate part, with white space always maintained between it and the main part of the glyph. This is a key feature that must also be maintained when the glyph is hinted and rendered on-screen, particularly at small sizes. 

<img width="100%" height="100%" src="Images/iillustration.png">

**Top:** In the top example here, when hinting is applied by the Autohinter, there is no stragety in place to keep this critical white space open and clear. The autohinter finds the top of the dot on the ‘i’ as a y-extreme, moves this point to the grid, and then misses controlling the weight of the dot. When the top of the dot on the ‘i’, is rounded to the grid, this rounding happens independantly from the hinting on the main stem of the ‘i’. This can cause the dot in some cases, as shown here in the condenced Bold, to round down to the grid, and clash with the ‘i’ stem, causing the glyph to look more like a lowercase ‘l’, than an ‘i’. 

**Bottom:** Showing the hinted glyph, kept open and clear. Note the Bold condensed Version in the Variation Window.

With a hinting stragety in mind, to always keep this white space open, it is easy to use some simple code to maintain a clear, open and readable glyph at all sizes for all variations. 

**Edit the Hinting of the ‘i’ to keep dot clear**

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/Hintiopen.gif)

This example shows how powerful VTT is, in allowing for some simple edits and code, to solve this screen rendering problem, for all variation of the glyph. 

To edit the autohinter output for the lowercase ‘i’ dot. Right click on point 4 at the top of the dot and drag to the left to remove the YAnchor command. **Note** _When any edits are made with the graphical hinting tools, the comments that are added by the Autohinter in the VTTtalk are removed, making it easier to view, and edit this code._

Once this edit is made the remaining code for the main stem of the ‘i’, remains, and is perfectly hinted, with the baseline point 2, anchored to the lowercase baseline cvt:10, and the top, point 3 anchored to the x-height cvt:6

The only code that is now needed is the critical code to mantain the the white space between the base stem and the dot of the ‘i’. Choose the YLink tool, drag from point 3 to to point 10 at the bottom of the ‘i’ dot, and make sure that the minimum distance is choosen. The following code is generated in the VTTtalk window.

**YDist(3,10 >=)**

The ‘>=’ in this command, ensures that there is always a minimum distance of 1 pixel maintained between point 6 at the lowercase square x-height to the bottom of the dot on the ‘i’. The bottom of the dot is now also rounded to the grid, maintaining contrast. Now choose the shift tool, and drag from point 10 to point 4 to control the weight of the dot. 

That is the hinting completed for the lowercase ‘i’. The dot will be kept at the correct distance from the main stem of the ‘i’, and critically, the white space will be maintained for all weights and variations of the glyph. Simple and powerful! This same stragety can be used to keep similar glyphs with similar features open and readable at all sizes. 

## Editing other lowercase glyphs

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/Hintlowercase.gif)

Once the control glyphs have been reviewed, and edited, hinting can progress on rest of the lowercase glyphs. As each glyphs is reviewed to ensure the correct cvt’s are assigned and the hinting structure overall, as output by the autohinter is correct, small edits can then be done to each glyph to fine tune the results. 

Note in the animation, only smaller edits are made, to shift untouched points and to add some interpolations. In some glyphs the autohinter adds hints, that are not needed, such as in the lowercase ‘r’. The shift from point 6 to point 3, is not needed, and can be removed, by a click and drag on the arrowhead of the shift to point 3. 



## Hinting complex glyphs

**Hinting strategy for Yen**

1. Control the top (Capital Height) and bottom (Baseline) to be consistent with other figure glyphs, using values in the Control Value Table as a reference. Minimise blur at the Cap Height.
 
2. Control the position of the two middle horizontal bars, in relation to the baseline and Capital height. 

3. Control the weight of the middle horizonal bars.

4. Maintain white space between middle bars in all variations.

The Autohinter is able to do 1 2 and 3, but has no stragety for 4. Maintaining white space is critical to ensure the glyph is kept open and readable. It is often easier and faster to rehint more complex glyphs from scratch, with a clear stragety in mind, rather than edit the autohinter code.

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/HintYen.gif)

**Step 1: Baseline and Cap Height Control** 

Choose the YShift Tool from the Toolbar. Position the ‘blue circle’, directly over point 12, and right click. With the right mouse button held down, drag to the right to select ‘round to grid’ then release. The Yen is defined in the  ‘Figure’ category, and the baseline cvt for the figure category will be choosen automatically. 

**YAnchor (12, 12)** Moves point 12 to the control value listed in the ‘Control Program’, that corresponds to the Figure baseline, (cvt #12) and rounds this point to a grid line.

**YShift(21,4)** Using the YShift Tool position the ‘blue circle’, directly over point 21, and drag to point 1. Moves point 21 to the control value listed in the ‘Control Program’, that corresponds to the Figure square height, (cvt #4) and rounds this point to a grid line. 

**YShift(21,1)**
Shifts point 1, to a new position on the grid, relative to point 21’s new position on the grid, ensuring point 1 will also be grid-fit to the figure height.

**Step 2: Position middle bars** 

**YIPAnchor(12,17,21)** Choose the YInterpolate tool. Position the ‘blue circle’, directly over point 21, at the top left, and drag to point 12 at the baseline. Click anywhere on the interpolaton line, and drag to the point you want to interpolate, in this case point 17 and release. Right click on point 17 and drag to the right to round the point to grid. It is important to choose the bottom point of the top middle bar, to begin the positioning, as this will be used to ensure the white space is preserved between the middle bars. This will also place point point 17 on a full pixel boundary ensuring that one side of the bar is kept on a high contrast grid boundary.

**YShift(17,6)** YShift from point 17 to point 6, to ensure point 6 is aligned with point 17

**YShift(17,20)** YShift from point 17 to point 20, to control the weight of the top middle bar.

**YShift(20,3)** YShift from point 20 to point 3, to ensure point 3 is aligned with point 20

**YShift(3,0)** YShift from point 3 to point 0, to position point 0 correctly in relation to the middle bar.

**Step 3: Maintain White Space** 

This is the critical step in the hinting process, to ensure that there is always white space maintained between the two middle bars, and to keep at least one side of the lower bar on a high contrast grid boundary. 

**YDist(17,16,>=)** Choose the YLink Tool from the Toolbar. Position the ‘blue circle’, directly over point 17, and drag to point 16 and release. Right click on the green arrowhead and drag down to ensure the minimum distance option is choosen. 

**>=** The greater than and equals sign appears after the 16 in the YDist command. This means, always keep a minimum distance of at least one pixel, ensuring in this case that there will always now be at least one pixel of white space between the two middle horizonal bars. This ensures that the glyph will appear open and readable at all sizes for all variations. This same stragety can be used for similar complex glyphs, in the currency range, or for other glyphs with multiple horizonals. 

**Note** _The YLink tool can generate two type of commands, a YLink, that uses a cvt as a reference to control a distance or YDist that uses the outline measurement, and no cvt._

**YShift(16,7)** YShift from point 16 to point 7, to ensure point 7 is aligned with point 16

**YShift(16,13)** YShift from point 16 to point 13, to control the weight of the bottom middle bar.

**YShift(13,10)** YShift from point 13 to point 10, to ensure point 10 is aligned with point 13

The hinting for the Yen is now complete. The heights are set, middle bars correctly positioned, and white space maintained for all sizes and all variations. 

## Hinting Accent glyphs

**Hinting strategy for Acute**

1. Move the bottom of the glyph to align to the grid
 
2. Ensure hinted glyph is at least two pixels in height at all sizes for all variations.

The Autohinter has no special stragety for hinting accented glyphs. By locking the top and bottom of the accent to the grid, the autohinter code can cause accents to collapse to one pixel in height. A one pixel high Acute accent, may be mistaken for a dot accent.

To ensure the best readable solution, accents that need it, should be hinted to be a minimum of two pixels in height for all sizes. 

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/HintAcute.gif)

**Hinting the Acute (0xb4)**

Choose the YLink Tool from the Toolbar. Position the ‘blue circle’, directly over point 6, and drag to point 12. Two lines of code are generated in the VTT Talk Window. Right click on the green arrowhead below point 12, and drag down to ensure the minimum distance option is choosen. _If you have set the YLink tool previously to use a dist and minimum distance, this may be already set._

YAnchor(6,16)
YDist(6,12,>=)

VTT automatically generates a cvt, when anchoring point 6. This cvt is not required, as accents are positioned later over the base glyphs using different positioning code. To remove the cvt, right click on the anchor symbol below point 6, drag to left to release. This sets point 6 to round to grid, but not use a cvt. The code will update to the following.

YAnchor(6)
YDist(6,12,>=)

Choose the YInterpolate tool, drag from point 6 to 12. Click anywhere on the interpolation line, and drag to point 1. Click again and drag to point 8.

YInterpolate(12,1,6,8)

The graphical hinting is now done. To ensure that the accent is at least two pixels tall at all sizes, move to the VTTalk Window. (`ctrl + 5`) 

Add 2 after the >= and compile the VTTTalk. (`ctrl + r`). This instructs the hinting to maintain a minimum distance of 2 pixels.

YDist(6,12,>=2)

The final code will look like this.

YAnchor(6)

YDist(6,12,>=2)

YInterpolate(12,1,6,8)

Smooth()

The acute accent now has the correct hinting, and ready for proofing. The accent will appear to be at least two pixels tall at all sizes, making it readable and clear for all variations.


**Hinting strategy for Tilde**

1. Move the bottom of the glyph to align to the grid
 
2. Ensure hinted glyph is at least two pixels in height at all sizes for all variations.

3. Preserve the correct shape of the glyph
 
![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/HintTilde.gif)

**Hinting the Tilde (0x2dc)**

Choose the YLink Tool from the Toolbar. Position the ‘blue circle’, directly over point 17, and drag to point 5. As in the previous example, ensure the YDist is set to use a mimimum distance. This can be checked by clicking on the arrowhead or viewing the generated code in the VTTTalk window. If mimimum distance is set a >= will appear after the YDIST command.

YAnchor(17,16)

YDist(17,5,>=)

Choose the YShift Tool from the Toolbar. Drag from point 5 to point 13, and from point 17 to 25, to align points 13 and 25 relative to points, 5 and 17. 

YShift(5,13)

YShift(17,25)

Right click on the anchor symbol below point 17 to remove the height cvt that was auto generated by VTT. 

YAnchor(17)

Choose the YInterpolate tool, drag from point 17 to 5. Click anywhere on the interpolation line, and drag to point 22. Click again and drag to point 10. _Note: There are different ways to hint any glyph. Another stragety for the Tilde would be to control the weight of the glyph, by using a shift command from point 17 to point 10, and from point 5 to 22. This will also work. This will depend on the design of the glyph and what type of variations are in the font. For OpenSans, the interpolation technique, works well to control the weight from light to black._

YInterpolate(5,10,22,17)

_Optional hinting step: Right click on point 5, and drag from Round to Grid, four spaces to the right to choose round up to grid. This command instructs the outline to round up to grid, and can produce better results in the heavier variations. By rounding up to grid, the glyph height can increase overall. More pixel can help in describing the shape of the tilde in the heavier weight variations._

YUpToGrid(5)

The graphical hinting is now complete. To ensure that the accent is at least two pixels tall at all sizes, move to the VTTalk Window. (`ctrl + 5`) 

Add 2 after the >= in the YDist(17,5,>=) command and compile the VTTTalk. (`ctrl + r`). This instructs the hinting to maintain a minimum distance of 2 pixels.

The final code for the Tilde in the VTTTalk window will look like this.

YAnchor(17)

YShift(17,25)

YUpToGrid(5)

YDist(17,5,>=2)

YShift(5,13)

YInterpolate(5,10,22,17)

Smooth()












