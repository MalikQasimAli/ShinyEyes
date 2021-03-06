# ShinyEyes

You can use this application at [https://colinquirk.shinyapps.io/shinyeyes/](https://colinquirk.shinyapps.io/shinyeyes/).

This is a shiny application for manually coding eyetracking data into catagories e.g. fixations, saccades, blinks, etc. Right now, you can name up to (any) three catagories as that's how many I need but if anyone out there has different needs let me know. The goal is to require very little in terms of requirements from the user. The goal is not to create an entire workflow, but is instead to provide a tool for people who know what they are doing. The input is simply a csv file that defines some necessary data and the output is a csv file with all of the windows that you've created.

![example.gif](example.gif)

## Installation

Currently, the only way to use ShinyEyes is to clone this repo or download the source code, then run the code yourself. I would like to change this one day, either by making this app a standalone website or by creating executables that can be run without having to have R installed separately. For now, you will need `R` (and optionally RStudio), and the following packages installed:

- `shiny`
- `tidyverse`
- `gganimate`
- `gifski`
- `png`

## Input data

Shiny eyes requires a csv with columns that define the following information:

- Grouping variables - what columns should be used to split your data up into chunks? For me, this is a `subject` and `trial` column, but you could have an arbitrary column splitting your data up into 5 seccond chunks if you had a continuous task, for example.
- Sample variable - what (single) column defines the timepoint for each of the samples in your data. In the testdata csv file, this is just sample.
- Gaze X variable - What (single) column defines the horizontal position of the gaze. I expect this will be in pixels, but strictly speaking it doesn't have to be.
- Gaze Y variable - What (single) column defines the vertical position of the gaze. I expect this will be in pixels, but strictly speaking it doesn't have to be.

## Usage

In addition to providing a csv file using the "Data File" file selector with the columns described above, you must also describe what you want your windows to be named:

- Window name 1 - What do you want to name your first window (associated with the keypress "1"). "Fixation" by default.
- Window name 2 - What do you want to name your second window (associated with the keypress "2"). "Saccade" by default.
- Window name 3 - What do you want to name your third window (associated with the keypress "3"). "Blink" by default.

You must also provide some reasonable scale if you are going to be looking at the gaze gifs:

- Gaze Gif X Scale Min - The minimum value on the X scale
- Gaze Gif X Scale Max - The maximum value on the X scale
- Gaze Gif Y Scale Min - The minimum value on the Y scale
- Gaze Gif Y Scale Max - The maximum value on the Y scale

By default, the settings are defined for a standard 1080p screen. You also need to provide a number of frames for your gif. 18 gives a reasonable amount of resolution without making the rendering take forever, but if your data has a different number of samples or if you would like to see more detail you may want to change this to be higher or lower.

When all of the necessary information has been provided, you will see a trace plot rendered along with the chunk information (for the test data, this would be subject and trial information). Make sure that each chunk only makes up a single piece of recorded data. The trace plots will not look correct if, for example, you did not include a subject column as a grouping variable despite having multiple subjects in your file. This trace plot can be interacted with by clicking and dragging to create a "window." To "lock in" a window, *you must press the "1", "2", or "3" key.* Your window will now be rendered on the actual plot (this is how you will know it is being stored). The sample information associated with the window will then be saved behind the scenes.

The `Undo Window` and `Clear Windows` buttons have similar functions. `Undo Window` will remove a single (saved) window and `Clear Windows` will remove all the windows saved for that chunk. These buttons will not effect any of the other chunks.

You can also delete a single window with the "d" key. The window your cursor is hovering over will be deleted. This is especially usefully when working with windows that have been imported in from an outside source.

The `previous chunk` and `next chunk` buttons will move you to the next (or previous) unique chunk in your data. Any windows from the last chunk are finilalized and stored. *If you return to a previous chunk, any old stored windows will be cleared.* This is currently the only way of going back and clearing window data that has already been stored.

There is also a `Skip Ahead` button which allows you to skip ahead any number of trials that you wish. By default, it will jump ahead 5% of your data, but you can change this value with the `Percent to skip (0-100)` option.

If you would like to view the gaze data in gif format, you can click the `Render Gaze Gif` function. This function uses the `gganimate` package which requires you to install the `gifski` and `png` packages (gganimate will not install them for you). I recommend clicking the `Hide Gaze Gif` button whenever you are not using this function as it will take a few seconds to render for each chunk, and therefore can be quite slow.

When you have finished creating your windows, use the `Download Data` button in order to save the data from your windows.

## Output Data

The output data will have the following columns where each row defines a window:

- Your grouping variables, as you defined
- xmin - the leftmost bound of the window in `sample variable` units
- xmax - the rightmost bound of the window in `sample variable` units
- window_name - Your defined name for the window, e.g. "Fixation"

## Importing Existing Codes

It can be time consuming to manually code all of your data from scratch if you have a large number of chunks to examine. To speed up this process, data can be preprocessed using whatever your preferred automatic system is and then imported into ShinyEyes. ShinyEyes expects the windows to be defined in the same way they are output. This means you can easily look at data you have previously coded with ShinyEyes, but data from other systems may require a decent amount of preprocessing.

To use this feature, import your data as described above. Then, import your codes using the "Import Existing Codes" field. This should be a link to a csv containing your grouping variables, an `xmin` column, an `xmax` column, and a `window_name` column.

## Contributing

If this app is of any use to you, send me a message (cquirk@uchicago.edu; @ColinTQuirk) to let me know so that I know to keep working on this project. I'm also happy to help with any difficulty you have when using this app. If you know much about eyetracking I would also love to hear about your experience, this is a somewhat new technique for me and I'm not sure if anyone even manually codes their data.

If you find a bug or have a feature request, feel free to open an issue or submit a pull request. 
