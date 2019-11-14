---
id: propertiesBackgroundAndBorder
title: Background and Border
---

## Alternate Background Color

Allows setting a different background color for odd-numbered rows/columns in a list box. By default, *Automatic* is selected: the column uses the alternate background color set at the list box level.

#### JSON Grammar

|Name|Data Type|Possible Values|
|---|---|---|
|alternateFill|string|any css value; "transparent"; "automatic"|

#### Objects Supported
[List Box](listbox_overview.md#overview)




## Background Color / Fill Color

Defines the background color of an object. 

In the case of a list box, by default *Automatic* is selected: the column uses the background color set at the list box level.

#### JSON Grammar


|Name|Data Type|Possible Values|
|---|---|---|
|fill|string|any css value; "transparent"; "automatic"|

#### Objects Supported

[Rectangle](shapes_overview.md#rectangle) - [Hierarchical List](list_overview.md#overview) - [List Box](listbox_overview.md#overview) - [List Box Column](listbox_overview.md#list-box-columns) - [List Box Header](listbox_overview.md#list-box-footers)

#### See also
[Transparent](#transparent)



## Background Color Expression

`Selection and collection type list boxes`

An expression or a variable (array variables cannot be used) to apply a custom background color to each row of the list box. The expression or variable will be evaluated for each row displayed and must return a RGB color value. For more information, refer to the description of the `OBJECT SET RGB COLORS` command in the *4D Language Reference manual*.

>With collection or entity selection type list boxes, this property can also be set using a [Meta Info Expression](properties_Text.md#meta-info-expression).

#### JSON Grammar

|Name|Data Type|Possible Values|
|---|---|---|
|rowFillSource|string|An expression returning a RGB color value|

#### Objects Supported
[List Box](listbox_overview.md#overview) - [List Box Column](listbox_overview.md#list-box-columns)







## Border Line Style / Dotted Line Type

Allows setting a standard style for the object border.

#### JSON Grammar

|Name|Data Type|Possible Values|
|---|---|---|
|borderStyle|text |"system", "none", "solid", "dotted", "raised", "sunken", "double"|

#### Objects Supported

[Picture Button](pictureButton_overview.md) - [Button Grid](buttonGrid_overview.md) - [Picture Pop-up Menu](picturePopupMenu_overview.md) - [Rectangle](shapes_overview.md#rectangle) - [Plug-in Area](pluginArea_overview.md#overview) - [Subform](subform_overview.md#overview) - [Web Area](webArea_overview.md#overview) - [Hierarchical List](list_overview.md#overview) - [List Box](listbox_overview.md#overview) - [4D Write Pro areas](writeProArea_overview.md)





## Dotted Line Type

Describes dotted line type as a sequence of black and white points. 

#### JSON Grammar

|Name|Data Type|Possible Values|
|---|---|---|
|strokeDashArray|number array or string|Ex. "6 1" or \[6,1\] for a sequence of 6 black point and 1 white point|

#### Objects Supported

[Rectangle](shapes_overview.md#rectangle) - [Oval](shapes_overview.md#oval) - [Line](shapes_overview.md#line)





## Hide extra blank rows

Controls the display of extra blank rows added at the bottom of a list box object. By default, 4D adds such extra rows to fill the empty area:

![](assets/en/FormObjects/property_hideExtraBlankRows1.png)

You can remove these empty rows by selecting this option. The bottom of the list box object is then left blank:

![](assets/en/FormObjects/property_hideExtraBlankRows2.png)

#### JSON Grammar

|Name|Data Type|Possible Values|
|---|---|---|
|hideExtraBlankRows|boolean|true, false|

#### Objects Supported

[List Box](listbox_overview.md#overview)





## Font Color / Line Color

For objects containing text (text, input, group box, list box header or footer), designates the font color.

For shape objects (rectangle, line, oval), designates the color of the object's lines.

#### JSON Grammar


|Name|Data Type|Possible Values|
|---|---|---|
|stroke |string |any css value, "transparent", "automatic"|

#### Objects Supported

[Group box](groupBox.md) - [Text](text.md) - 
[Input](input_overview.md) - [List box header](listbox_overview.md#header) - [List box footer](listbox_overview.md#footer) - [Rectangle](shapes_overview.md#rectangle) - [Oval](shapes_overview.md#oval) - [Line](shapes_overview.md#line)




## Line Width

Designates the thickness of a line.

#### JSON Grammar

|Name|Data Type|Possible Values|
|---|---|---|
|strokeWidth|number|0 for smallest width on a printed form, or any integer value < 20|

#### Objects Supported

[Rectangle](shapes_overview.md#rectangle) - [Oval](shapes_overview.md#oval) - [Line](shapes_overview.md#line)








## Row Background Color Array

`Array type list boxes`

The name of an array to apply a custom background color to each row of the list box or column.

The name of a Longint array must be entered. Each element of this array corresponds to a row of the list box (if applied to the list box) or to a cell of the column (if applied to a column), so the array must be the same size as the array associated with the column. You can use the constants of the [SET RGB COLORS](https://doc.4d.com/4Dv17R6/4D/17-R6/SET-RGB-COLORS.302-4310385.en.html) theme. If you want the cell to inherit the background color defined at the higher level, pass the value -255 to the corresponding array element.

For example, given a list box where the rows have an alternating gray/light gray color, defined in the properties of the list box. A background color array has also been set for the list box in order to switch the color of rows where at least one value is negative to light orange:

```code4d
 <>_BgndColors{$i}:=0x00FFD0B0 // orange
 <>_BgndColors{$i}:=-255 // default value
```
![](assets/en/FormObjects/listbox_styles1.png)

Next you want to color the cells with negative values in dark orange. To do this, you set a background color array for each column, for example <>_BgndColor_1, <>_BgndColor_2 and <>_BgndColor_3. The values of these arrays have priority over the ones set in the list box properties as well as those of the general background color array:

```code4d
 <>_BgndColorsCol_3{2}:=0x00FF8000 // dark orange
 <>_BgndColorsCol_2{5}:=0x00FF8000
 <>_BgndColorsCol_1{9}:=0x00FF8000
 <>_BgndColorsCol_1{16}:=0x00FF8000
```
![](assets/en/FormObjects/listbox_styles2.png)

You can get the same result using the `LISTBOX SET ROW FONT STYLE` and `LISTBOX SET ROW COLOR` commands. They have the advantage of letting you skip having to predefine style/color arrays for the columns: instead they are created dynamically by the commands.


#### JSON Grammar

|Name|Data Type|Possible Values|
|---|---|---|
|rowFillSource|string|The name of a longint array.|

#### Objects Supported
[List Box](listbox_overview.md) - [List Box Column](listbox_overview.md#list-box-columns)






## Transparent

Sets the list box background to "Transparent". When set, any [alternate background color](#alternate-background-color) or [background color](#background-color-fill-color) defined for the column is ignored. 

#### JSON Grammar

|Name|Data Type|Possible Values|
|---|---|---|
|fill|text|"transparent"|

#### Objects Supported
[List Box](listbox_overview.md#overview)

#### See also
[Background Color / Fill Color](#background-color-fill-color)