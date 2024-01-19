<h1> <img src="https://raw.githubusercontent.com/LiveSplit/LiveSplit/master/LiveSplit/Resources/Icon.png" alt="LiveSplit" height="42" width="45" align="top"/> LiveSplit</h1>

LiveSplit is a timer program for speedrunners that is both easy to use and full of features.

This is a fork of LiveSplit that adds functionality to start at any desired split with a desired initial time. This was designed in mind for usage by SMW/Kaizo players. In conjunction with my [ExitCounter component](https://github.com/protopizza/LiveSplit.ExitCounter), it makes timing your progress of a first playthrough of any game easy to manage over multiple gaming sessions. It's how I track my time over on my stream at https://www.twitch.tv/protopizza.

<img src="https://raw.githubusercontent.com/protopizza/LiveSplit/master/LiveSplit/Resources/KaizoSplit/Guide-1.png"/>

<img src="https://raw.githubusercontent.com/protopizza/LiveSplit/master/LiveSplit/Resources/KaizoSplit/Guide-2.png"/>

You can see the main project and documentation for LiveSplit [here](https://github.com/LiveSplit/LiveSplit).

## How-To

### Setup

Download the .zip file from the releases and then extract it to get this LiveSplit fork.

If you want, you can take only the files `LiveSplit.exe`, `LiveSplit.Core.dll`, and `LiveSplit.View.dll` and replace them in your existing LiveSplit installation, which needs to be version `1.8.0`.

You may also want to get the `ExitCounter` component [here](https://github.com/protopizza/LiveSplit.ExitCounter), which has its own documentation.

### Usage

In normal usage, you'll want to keep `Auto Start Timer` and `Auto Segment Index` checked within the Splits Editor. When you are done playing for any particular session, **pause** LiveSplit, then right-click on LiveSplit and hit `Save Splits`. You will be prompted to save it as a Personal Best, for which you should click `Yes`.

I normally then go back and add in the names of the Segments based on the exits I played.

Further details of the features below.

### Features

1. Automatically setting your start timer. This is useful for when playing across multiple sessions, and you want to start the timer of your next session at the time your previous session left off at, automatically, by just checking `Auto Start Timer` in the Splits Editor (defaults to checked).
    1. You can also uncheck this in order to start at any arbitrary total time, in case you want to start with already 30 minutes on the clock for a given exit.
1. Starting your run from a segment in the middle of your splits. This is useful to keep the past segment history maintained over the course of your play. If you check `Auto Segment Index`, this automatically starts you from the latest split without a recorded split time (defaults to checked).
    1. **Note:** The first segment is 0, not 1. The second one is 1, third is 2, etc.
1. For playing games with a lot of exits, it's annoying to have to set up a ton of segments for each. (JUMP½ has 130!) This is made easy by entering in a value for `Total Desired Segments`, then hitting the `Populate Segments` button.

### Other Notes

1. This only works using the "Real Time" method of timing.
1. Settings based around the `Auto Start Timer`, `Auto Segment Index`, and `Start Segment Index` aren't saved across closing and opening the program, as they don't get stored into your actual splits files. This allows your splits files to be compatible with the main version of LiveSplit.

## Compiling

LiveSplit is written in C# 7 with Visual Studio and uses .NET Framework 4.6.1. To compile LiveSplit, you need one of these versions of Visual Studio:
 - Visual Studio 2017 Community Edition
 - Visual Studio 2017

Simply open the project with Visual Studio and it should be able to compile and run without any further configuration.

## Common Compiling Issues
1. Could not build Codaxy.Xlio due to sgen.exe not being found. Open LiveSplit\\Libs\\xlio\\Source\\Codaxy.Xlio\\Codaxy.Xlio.csproj in order to edit where it looks for this path. Look for &lt;SGen...&gt; where it defines the attribute "ToolPath". Look on your computer to find the proper path. It is typically down some path such as "C:\\Program Files (x86)\\Microsoft SDKs\\Windows\\x.xA...". Find the version you want to use and bin folder with sgen.exe in it and replace the path in the .csproj file.
2. No submodules pulled in when you fork/clone the repo which causes the project not to build. There are two ways to remedy this:
 - Cloning for the first time: `git clone --recursive git://repo/repo.git`
 - If already cloned, execute this in the root directory: `git submodule update --init --recursive`

## Auto Splitters

The documentation for how to develop, test, and submit an Auto Splitter can be found here:

[Auto Splitters Documentation](https://github.com/LiveSplit/LiveSplit.AutoSplitters/blob/master/README.md)

## Releasing

1. Update versions of any components that changed (create a Git tag and update the factory file for each component) to match the new LiveSplit version.
2. Create a Git tag for the new version.
3. Download `LiveSplit_Build` from the GitHub Actions build for the latest commit on `master`.
4. Create a GitHub release for the new version, and upload the LiveSplit build ZIP file with the correct filename (e.g. `LiveSplit_1.8.21.zip`).
    - Create releases for [`LiveSplit.Counter`](https://github.com/LiveSplit/LiveSplit.Counter) and [`LiveSplit.Server`](https://github.com/LiveSplit/LiveSplit.Server) if necessary.
5. Modify files in [the update folder of LiveSplit.github.io](https://github.com/LiveSplit/LiveSplit.github.io/tree/master/update) and commit the changes:
    - Copy changed files from the downloaded LiveSplit build ZIP file to the update folder.
    - Add new versions to the update XMLs for (`update.xml`, `update.updater.xml`, and the update XMLs for any components that changed).
    - Update the version on the [downloads page](https://github.com/LiveSplit/LiveSplit.github.io/blob/master/downloads.md).

## License

The MIT License (MIT)

Copyright (c) 2013 Christopher Serr and Sergey Papushin

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
