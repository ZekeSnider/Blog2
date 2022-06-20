---
layout: page
name: Chronolyzer
excerpt_separator: <!--more-->
---

Chronolyzer is a timestamp utility for macOS. It allows you easily convert between different timestamp formats, and customize the display to your use case.

{% include fullWidthImage.html file="ChronolyzerMacbookAir.png"
alt="Chronolyzer running on a Macbook Air" description="" %}

<!--more-->

Features:
+ Easily convert between different date formats
+ Auto-detect the format of imported timestamps
+ Quickly copy formatted timestamps to the clipboard
+ Add custom date format configurations
+ Configure different timezones
+ Immediately set the timestamp to the current time, or auto update once per second
+ [VoiceOver](https://support.apple.com/guide/voiceover/welcome/mac) fully supported

<a href="https://apps.apple.com/us/app/chronolyzer/id1556591076">
{% include notfullWidthImageLeftJustify.html file="macAppStore.svg" alt="Download on Mac App Store glyph" description="" %}
</a>

_Requires macOS 11 or later. Supports Intel and Apple Silicon Macs._

## Documentation

### Setting to current time
To update to the current time, click the clock icon in the top right, or use the keyboard shortcut `⌘ R`. 

To have the timestamp automatically update once per second to the current timestamp, use the `Auto Update` toggle at the bottom of the page.

### Importing a Timestamp
To import a timestamp, click the import button in the toolbar, or use the keyboard shortcut `⌘ I`. 

From the import view, you can paste your timestamp, and choose the desired format. 

+ Auto

This option will attempt to automatically detect the format of your timestamp, based on a set of presets. It will display the matched format below the input box. If it does not match correctly, you will need to select another option from the dropdown.

+ Date Picker

This option allows you to the use the system's datepicker to input a date. You may also select the timezone of the date. It defaults to your mac's local timezone.

+ Seconds / Milliseconds / Microseconds

This option allows you to enter an integer of epoch time.

+ Date Format / Custom

There are a number of preset date formats you can choose from. Additionally, if you choose custom, you may enter your own format. For more info, see "Custom Date Formats" below.


## Preferences
{% include fullWidthImage.html file="chronolyzer/PreferencesMenu.png" %}
In Chronolyzer preferences, you can add or remove Date Formats used in the main window, and manage other settings. To access the preferences pane, use `⌘ ,`, or navigate to Chronolyzer -> Preferences in the Menu Bar.

### Adding date formats
{% include fullWidthImage.html file="chronolyzer/AddNewDateFormat.png" %}

To add new date formats, use the `Add New` button. Currently, the following options are supported:

+ Date Picker

macOS's provided date display, with an added seconds field. You may also specify a timezone, or it will default to the local timezone.

+ Epoch Time

Displays time in [Unix epoch time](https://en.wikipedia.org/wiki/Unix_time), the duration since `00:00:00 UTC on 1 January 1970`. This view allows you to switch to displaying the time in Seconds, Milliseconds, or Microseconds.

+ Formatted String

Displays time using the specified NSDateFormatter string. You may also specify a timezone, or it will default to the local timezone

### Removing date formats
{% include fullWidthImage.html file="chronolyzer/DeleteDateFormat.png" %}

To remove formats, right click the desired row in the table, then click `Delete` from the context menu.

### Other settings
+ Automatically paste when importing

If enabled, the text field in the import view will be pre-filled with your clipboard's contents when opened.

## Custom Date Formats
The date format used by all custom fields is [NSDateFormatter](https://developer.apple.com/documentation/foundation/nsdateformatter). For a useful guide to the syntax, see [nsdateformatter.com](https://nsdateformatter.com).

## Support
Chronolyzer is a free app that is provided without any warranty. That said, if you run into issues or have feature suggestions, please [let me know](/contact).

## Privacy
No information is collected from Chronolyzer. The app only handles timestamps, and no data leaves your device. Your clipboard can be used to enable app features, if desired.