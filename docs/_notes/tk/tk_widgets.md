# TK Widgets

A basic component of TK based applications. Here widgets and windows are used interchangeably. It gives a graphical component to creating graphical applications.

Widgets range from buttons to menus and data displays. Widgets are very configurable and are easy to use.

Widgets follow a hierarchy system where widgets are able to be placed inside another widget. The main widget will be known as the root widget, and can be created by making a new instance of the TkRoot class.

## Creating a Widget

Here is the syntax:

```tcl
type variableName arguments option
```

- **`type`** refers to the widget type, like `button`, `label`, etc. Arguments can be optional and required bases on the type, and the option ranges from size to formatting of each component.

## Widget Naming convention

Widget uses a structure similar to naming packages. In TK, the root window is named with a period (.) and an element in window, for example button is named .myButton1. The variable name should start with a lowercase letter, digit, or punctuation mark (except period). After the first character, other character maybe uppercase or lowercase letters, numbers, or punctuation marks (except periods). Lowercase letters are the recommended starting character.

## Color Naming Convention

You can either use English names for them, or you can hexadecimals with #. Your hexadecimal values have to be a multiple of 3 from 3 to 12.

## Dimension Convention

The default unit is pixels and it is used when we specify no dimension. The other dimension are i for inch, m for milimeters, c for centrimeters, and p for points.

## Common Options

| Syntax                | Description                                                                                                  |
| --------------------- | ------------------------------------------------------------------------------------------------------------ |
| -background color     | used to set background color for widget                                                                      |
| -borderwidth width    | Used to draw with border in 3D effect                                                                        |
| -font fontDescriptor  | Used to set font for widget                                                                                  |
| -foreground color     | Used to set foreground color for widget                                                                      |
| -height number        | Used to set height for widget                                                                                |
| -highlightcolor color | Used to set the color rectangle to draw around a widget when the widget has input force                      |
| -padx number          | Sets the padx for the widget                                                                                 |
| -pady number          | Sets the pady for the widget                                                                                 |
| -relief condition     | Set the 3D relief for this widget. The condition may be raised, sunken, flat, ridge, solid, or groove        |
| -text text            | Sets the text for the widget                                                                                 |
| -textvariable varName | Variable associated with the widget. When the text of the widget changes, then this variable changes with it |
| -width number         | Sets the width of the widget.                                                                                |


## Widget types

### Basic Widgets

| Widget               | Description                                                            |
| -------------------- | ---------------------------------------------------------------------- |
| [Label](tk_label.md) | Widget for displaying a single line of text                            |
| Button               | Widget that is clickable and triggers an action                        |
| Entry                | Widget used to accept a single line of text input                      |
| Message              | Widget for displaying multiple lines of texts                          |
| Text                 | Widget for displaying and optionally edit multiple lines of texts      |
| Toplevel             | Window with all borders and decorations provided by the Window Manager |

### Layout Widget

| Widget      | Description                                                                       |
| ----------- | --------------------------------------------------------------------------------- |
| Radiobutton | Widget that has a set of on/off buttons and labels, one of which may be selected  |
| Checkbutton | Widget that has a set of on/off buttons and labels, many of which can be selected |
| Menu        | Widget that acts as holder for menu items                                         |
| Listbox     | Widget that displays a list of cells, one or more of which may be selected        |


### Mega Widgets

| Widget      | Description                                                                            |
| ----------- | -------------------------------------------------------------------------------------- |
| Dialog      | Widget for displaying dialog boxes                                                     |
| Spinbox     | Widget that allows users to choose number                                              |
| Combobox    | Widget that combines an entry with a list of choices available to the user             |
| Notebook    | Tabbed widget that helps to switch between one of several pages, using an index tab    |
| Progressbar | Widget to provide visual feedback to the progress of long operations like file uploads |
| Treeview    | Widget to display and allow browsing through a hierarchy of items more in form of tree |
| Scrollbar   | Scrolling widgets without a text or canvas widget                                      |
| Scale       | Lets you choose a numeric value through a slider                                       |

### Other

| Widget | Description               |
| ------ | ------------------------- |
| Canvas | Drawing widget for images |


