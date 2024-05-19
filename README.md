<h1> <img src="https://raw.githubusercontent.com/LiveSplit/LiveSplit/master/res/Icon.svg" alt="LiveSplit" height="42" align="top"/> LiveSplit</h1>


This is a fork of LiveSplit that adds functionality to start at any desired split with a desired initial time. This was designed in mind for usage by SMW/Kaizo players. In conjunction with my [ExitCounter component](https://github.com/protopizza/LiveSplit.ExitCounter), it makes timing your progress of a first playthrough of any game easy to manage over multiple gaming sessions. It's how I track my time over on my stream at https://www.twitch.tv/protopizza.

<img src="https://raw.githubusercontent.com/protopizza/LiveSplit/master/res/KaizoSplit/Guide-1.png"/>

<img src="https://raw.githubusercontent.com/protopizza/LiveSplit/master/res/KaizoSplit/Guide-2.png"/>

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

 1. [Fork](https://github.com/LiveSplit/LiveSplit/fork) the project
 2. Clone your forked repo: `git clone --recursive https://github.com/YourUsername/LiveSplit.git`
 3. Create your feature/bugfix branch: `git checkout -b new-feature`
 4. Commit your changes to your new branch: `git commit -am 'Add a new feature'`
 5. Push to the branch: `git push origin new-feature`
 6. Create a new Pull Request!

## Compiling

LiveSplit uses .NET Framework 4.6.1. To compile LiveSplit, you need the following components installed:
- [.NET 8.0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
- [.NET Framework 4.6.1 Developer Pack](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net461)

After cloning, simply run `dotnet build LiveSplit.sln` from the root of the repository.

To use Visual Studio, you must install a version that supports the .NET SDK version you installed. At the time of writing, the most recent version is [Visual Studio 2022](https://visualstudio.microsoft.com/vs/community).

## Common Compiling Issues
1. No submodules pulled in when you fork/clone the repo which causes the project not to build. There are two ways to remedy this:
 - Cloning for the first time: `git clone --recursive https://github.com/LiveSplit/LiveSplit.git`
 - If already cloned, execute this in the root directory: `git submodule update --init --recursive`

## Auto Splitters

The documentation for how to develop, test, and submit an Auto Splitter can be found here:

[Auto Splitters Documentation](https://github.com/LiveSplit/LiveSplit.AutoSplitters/blob/master/README.md)

## The LiveSplit Server

The internal LiveSplit Server allows for other programs and other computers to control LiveSplit. The server can accept connections over either a named pipe located at `\\<hostname>\pipe\LiveSplit` (`.` is the hostname if the client and server are on the same computer) or over TCP/IP.

### Control

The named pipe is always open while LiveSplit is running but the TCP server **MUST** be started before programs can talk to it (Right click on LiveSplit -> Control -> Start TCP Server). You **MUST** manually start it each time you launch LiveSplit.

### Settings

#### Server Port

**Server Port** is the door (one of thousands) on your computer that this program sends data through. Default is 16834. This should be fine for most people, but depending on network configurations, some ports may be blocked. See also https://en.wikipedia.org/wiki/Port_%28computer_networking%29

### Known Uses

- **Android LiveSplit Remote**: https://github.com/Ekelbatzen/LiveSplit.Remote.Android
- **SplitNotes**: https://github.com/joelnir/SplitNotes
- **Autosplitter Remote Client**: https://github.com/RavenX8/LiveSplit.Server.Client

Made something cool? Consider getting it added to this list.

### Commands

Commands are case sensitive and end with a new line. You can provide parameters by using a space after the command and sending the parameters afterwards (`<command><space><parameters><newline>`).

Some commands will respond with data and some will not. Every response ends with a newline character.

All times and deltas returned by the server are formatted according to [C#'s Constant Format Specifier](https://learn.microsoft.com/en-us/dotnet/standard/base-types/standard-timespan-format-strings#the-constant-c-format-specifier). The server will accept times in the following format: `[-][[[d.]hh:]mm:]ss[.fffffff]`. The hours field can be greated than 23, even if days are present. Individual fields do not need to be padded with zeroes. Any command that returns a time or a string can return a single hyphen `-` to indicate a "null" or invalid value. Commands that take a COMPARISON or a NAME take plain strings that may include spaces. Because it is used as a delimiter to mark the end of a command, newline characters may not appear anywhere within a command.

Commands that generate no response:

- startorsplit
- split
- unsplit
- skipsplit
- pause
- resume
- reset
- starttimer
- setgametime TIME
- setloadingtimes TIME
- addloadingtimes TIME
- pausegametime
- unpausegametime
- alwayspausegametime
- setcomparison COMPARISON
- switchto realtime
- switchto gametime
- setsplitname INDEX NAME
- setcurrentsplitname NAME

Commands that return a time:

- getdelta
- getdelta COMPARISON
- getlastsplittime
- getcomparisonsplittime
- getcurrentrealtime
- getcurrentgametime
- getcurrenttime
- getfinaltime
- getfinaltime COMPARISON
- getpredictedtime COMPARISON
- getbestpossibletime

Commands that return an int:

- getsplitindex  
(returns -1 if the timer is not running)

Commands that return a string:

- getcurrentsplitname  
- getprevioussplitname
- getcurrenttimerphase
- ping  
(always returns `pong`)

Commands are defined at `ProcessMessage` in "CommandServer.cs".

### Example Clients

#### Python

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("localhost", 16834))
s.send(b"starttimer\n")
```

#### Java 7+

```java
import java.io.IOException;
import java.io.PrintWriter;
import java.net.Socket;

public class MainTest {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("localhost", 16834);
        PrintWriter writer = new PrintWriter(socket.getOutputStream());
        writer.println("starttimer");
        writer.flush();
        socket.close();
    }
}
```

#### Node.js

Node.js client implementation available here: https://github.com/satanch/node-livesplit-client

## Releasing

1. Update versions of any components that changed (create a Git tag and update the factory file for each component) to match the new LiveSplit version.
2. Create a Git tag for the new version.
3. Download `LiveSplit_Build` and `UpdateManagerExe` from the GitHub Actions build for the new Git tag.
4. Create a GitHub release for the new version, and upload the LiveSplit build ZIP file with the correct filename (e.g. `LiveSplit_1.8.21.zip`).
5. Modify files in [the update folder of LiveSplit.github.io](https://github.com/LiveSplit/LiveSplit.github.io/tree/master/update) and commit the changes:
    - Copy changed files from the downloaded LiveSplit build ZIP file to the [update folder](https://github.com/LiveSplit/LiveSplit.github.io/tree/master/update).
    - Copy changed files from the download Update Manager ZIP file to replace [`UpdateManagerV2.exe`](https://github.com/LiveSplit/LiveSplit.github.io/blob/master/update/UpdateManagerV2.exe) and [`UpdateManagerV2.exe.config`](https://github.com/LiveSplit/LiveSplit.github.io/blob/master/update/UpdateManagerV2.exe.config).
    - Add new versions to the update XMLs for (`update.xml`, `update.updater.xml`, and the update XMLs for any components that changed).
    - Modify the [DLL](https://github.com/therungg/LiveSplit.TheRun/blob/main/Components/LiveSplit.TheRun.dll) and [update XML](https://github.com/therungg/LiveSplit.TheRun/blob/main/update.LiveSplit.TheRun.xml) for LiveSplit.TheRun in its repo.
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
