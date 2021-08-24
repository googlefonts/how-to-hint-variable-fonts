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
 
When DirectWrite renders a font’s spacing, glyphs can now begin on a subpixel boundary. [Subpixel positioning](https://docs.microsoft.com/en-us/windows/win32/directwrite/introducing-directwrite#improved-text-rendering-with-cleartype) greatly improves the spacing of type on-screen, especially at small sizes. The spacing accuracy that can be achieved by starting a glyph’s spacing on a subpixel boundary is significant compared with starting on a whole pixel as in GDI. This more accurate spacing is critical in achieving a smooth flow and even typographic color to the text. In addition, hinting is no longer needed to control the font’s spacing, which in the past required a lot of extra code and time to complete.
 
**Horizontal and Vertical anti-aliasing**
 
Subpixel rendering improves the horizontal aspect of type on-screen but not the vertical. Older types of rendering, such as Cleartype in Windows GDI for example, only supported horizontal anti-aliasing. This meant that aliasing or ‘jaggies’ were still very apparent at larger sizes on-screen. In order to fix this problem and give a smoother vertical effect, DirectWrite applies [anti-aliasing in the y-direction](https://docs.microsoft.com/en-us/windows/win32/directwrite/introducing-directwrite#improved-text-rendering-with-cleartype) in addition to using the horizontal subpixel rendering. 
 
The addition of support for vertical anti-aliasing, helped a great deal in smoothing out the appearance of text on-screen, particularly at larger sizes. 

However, for smaller font sizes, when a font’s outlines are scaled and rendered with no hinting, the vertical anti-aliasing can result in significant blur, particularly for horizontal features. By aligning key heights and horizontal features to the pixel grid, hinting helps to greatly reduce this blur, in particular for text reading sizes, on lower resolution screens.

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

**NOTE:** _Refer also to the extensive Help file in VTT for additional information on setup, proofing, Hinting basics, Autohinting, Variable fonts, and more._

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

**GID number:** The Glyph ID for the currently selected glyph, in the sequential order the glyphs are stored in the font file. **Pro Tip:** The glyph order can be displayed in two ways, under `Tools > options > settings > Access glyph by Index`. When this checkbox is set, the character set (`ctrl-9`) will show the glyphs as they are ordered in the font file. It is useful to leave this as the default setting, as the hinting for every glyph in the font should be proofed and checked. If this option is unset, only glyphs with Unicode codepoints assigned will be shown. This is also a useful view, but if this is set as the default, some glyphs may be missed in the hinting and proofing process.

**Char:** If the glyph has an associated Unicode, this will be shown, eg: Cap H (0x48). If there is no Unicode associated with the glyph, this will be shown as Oxffff _(OpenType glyphs such as Small Caps, figure styles or ligatures, for example). Unicode is followed by the Glyph name.

**Uppercase:** Character group information for each character. The character group tells VTT which group of values to use from the Control Value Table.

**pt / ppem** Currently selected point/ppem size reflecting the selected resolution _(The resolution to display the text string and waterfall run in the main window can be changed (`Display > Resolution > choose desired resolution`). This is useful for proofing at different resolutions)_ Choose the size to view in the Main window, `Display > Size > ...`, or click on a size from the text ramp at the bottom of the Main Window. Use up and down arrows to change the point size, or `ctrl + equals`, and enter the desired size.

**grid-fitted:** Showing if hinting is turned on (grid-fitted) or off. To toggle between the two states use `ctrl + g`. 

**PRO TIP** `ctrl + g` is worn out on my keyboard. While adding hints or reviewing the autohinter code, `ctrl + g` switches between hinting and no hinting. It is critical to always keep the original outline in mind, _(no hinting)_ to ensure that there is minimal distortion between the two states. 

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

This window can be used to adjust the numeric value of particular control values for different variation instances. _(For example, editing the value of the x-height, control value, where the outline design is different from the default instance)_

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

A color-coded system is shown here as an example of marking each glyph as having final hinted code, being edited, and being proofed. In this example (Open Sans), there are four general categories for glyphs that need hinting and proofing.

1. **Unique glyphs** _(This can involve: Reviewing the Autohinter’s code, editing and fine tuning, or hinting from scratch.)_

2. **Pure composite glyphs** _(Composite of an original glyph with only offset commands in the glyph program. In this case, no code changes are necessary, only proofing. It is only the Unique reference glyph, which needs hinting)_

3. **Composites: Hinting code editing required.** Composite glyphs which use `USEMYMETRICS`, offset transformation and positioning code. At the time of writing, the code generated by the Autohinter does not work for variable fonts. The code calls a helper function (function 86) from the Font Program which uses an _outline measured distance_ as a reference for positioning accent glyphs in the y-direction. This procedure works for static fonts where there is only one measured distance, but in variable fonts, the distance between the accent and base glyph can vary. The code currently generated by the autohinter only positions the accent correctly for the default instance. 

If the measured outline distance between the base glyph and the accent is the same across the variation space, this code can be used. If the accent distance measurement varies, custom code is needed.

4. **Composites: Autohinter code output is ok**. If the autohinter's measured distance is constant, then the code is OK to use for a variable font.

<img width="100%" height="100%" src="Images/HintOS.png">



## VTT Variable font workflow process 

**Step 1.** Autohinting a Variable Font

VTT includes an Autohinter for Latin fonts. This Autohinter makes use of a lightweight hinting strategy that focuses on fitting common heights, such as x-height, cap height, ascender, descender to heights stored in the Control Value Table (CVT). This strategy takes advantage of Windows’ symmetric rendering modes and anchors these key heights to the grid, thereby maintaining consistency in heights across a font family. This method of grid-fitting also helps to reduce blur and minimizes distortion.

Follow these steps to Autohint a Latin font:

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/VTTLightLatinAutohinter.gif)

1. Start Visual True Type.
2. File > Open. Navigate to font file you would like to Autohint.
3. Select Font File and Open.
4. From the Tools menu, select Autohint > Light Latin Autohint.
5. When Autohinting is complete choose Save from File Menu

**Note** The intent is that this Autohinter works well enough for most glyphs. Autohinting for all glyphs in the font should be carefully checked and proofed, and certain glyphs may need to be re-hinted manually, either by using the Visual Hinting tools, or by editing the VTT Talk code directly._

## Hinting Concepts (Hinting the H & O)

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

**YAnchor (5, 8)** Moves point 5 to the control value listed in the ‘Control Program’, that corresponds to the baseline, (cvt #8) and rounds this point to a grid line. (View > Control Program:  (8: 0 /* base line */)

**YShift(5,1)** Choose the YShift Tool. Position the ‘blue circle’, directly over point 5, and drag to point 1. Shifts point 1, to a new position on the grid, relative to point 5’s new position on the grid, ensuring point 1 will also be grid-fit to the baseline.
 
**Note:** Because the capital ‘H’ is defined as ‘Uppercase’, VTT knows which group of values to use from the Control Value table, and will generate the correct values automatically.

**Cap Height Control**

Repeat Step 1, this time for point 6 at the Cap Height.

**YAnchor (6, 2)** Moves point 6 to the control value, that corresponds to the cap height, (cvt #2) and rounds this point to a grid line. (View > Control Program:  (2: 1462 /* cap height */)

**YShift(6,10)** Shifts point 10, to a new position on the grid, relative to point 6’s new position on the grid, ensuring point 10 will also be grid-fit to the cap height.

**Note:** _There are different methods that can be used in hinting. The best methods are efficient, using the least amount of code. An alternate method to using the shift command,is to YAnchor points 1 and 10, using cvt 8 and 2. The hinted result will be the same._ 

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

Moves point 3, to full pixel grid, relative to point 5 and point 6, which now has a new grid-fit position. This ensures high contrast on the bottom of the horizontal crossbar on the Cap H, and helps to minimise blur. _(The Autohinter also chooses point 3 on the bottom of the horizontal bar, to interpolate, between the top and bottom of the Cap H.)_

If point 8 was chosen, the bar would have a tendency to round down and may cause the bar to look optically low. It is better visually for the bar to be high as opposed to low, the same conceppt as is used in the high resolution design. This same hinting strategy can then be used in a consistent way, for other glyphs with a centre bar, capital ‘E’, ‘F’, etc.

**Step 4: Control the middle horizontal bar weight**

Choose the YShift tool. Position the blue circle over point 3, and drag to point 8. The following code is generated in the VTT Talk Window.

**YShift(3,8)** Shifts point 8, to a new position, relative to point 3’s new position on the grid, maintaining the same relative distance between the point 3 and point 8 as is in the original high resolution design of the outline. The shift command does not reference a cvt value and does not move the hinted point 8 to a full pixel grid line. Shift also does not default to a one pixel minimum used by the Link command. Using the Shift command will maintain a balanced visual weight, of horizontal features in particular, across all variations.

**Step 5: Adding the Res command** 

The Res addition to the command ResAnchor, for example, stands for Rendering Environment Specific, and ensures that the appropriate rounding happens, for the various rendering environments. This saves adding additional Hinting commands if Hinting is required to work in a variety of rendering environments._The Res command calls a Function, that is designed to also allow for more subtle rendering of features such as undershoots and overshoots

_( Please refer to the longer section on Res commands for more details.)_ **(link to this)**

Switch to the VTTtalk window** (`ctlr 5`). Type Res before the YAnchor commands. Compile VTT Talk, (`ctrl r`) and save (`ctrl + s`).

The ‘Res’ _(Resolution Environment Specific)_ command is not supported when using the Graphical Hinting tools. This additional code can be added manually, in the VTTtalk window. _(See the notes on Res command in the next section)_

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

**Hinting the Cap O**

![LatinAutohinter](https://github.com/googlefonts/how-to-vtt/blob/main/Images/HintO.gif)

**High level hinting strategy for hinting the uppercase O**

Control the top _(round overshoot height)_ and bottom _(round undershoot height)_ to be consistent with other uppercase glyphs, using values in the Control Value Table as a reference.
 
Reduce blur at the Capital Overshoot height, and baseline undershoot height, using *inheritence, to force the undershoot and overshoot values to be equal to the baseline and square cap height until a defined size

_*inhertence: A method of forcing one cvt to be equal to another cvt until a certain size. This is used for smaller point sizes on-screen to supress subtle features that cannot be shown when there is a linited number of pixels_
 
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
 
50 means that the overshoot should kick in at 50 ppem. Replace the 50ppem by whichever ppem size you wish the overshoot to kick in. The size at which the  undershoot and overshoots will kick in, is usually kept to be equal.
 
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


## Editing the Hinting

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









