# [J programming language](https://jsoftware.com)

Unlike [Dyalog APL (proprietary; best APL implementation)](https://www.dyalog.com/), 
J is completely open source.

[Install J locally](#install-j-locally) or run J online at: 

- [J Playground](https://code.jsoftware.com/wiki/Category:J_Playground_P): 
  https://jsoftware.github.io/j-playground/bin/html2/
- [Juno web IDE for J](https://jsoftware.github.io/juno/app/)

## TODO

- Add a page in https://learnxinyminutes.com for J, inspired by existing APL page.
- Translate [Learn APL](https://xpqz.github.io/learnapl) book to J.
- Add J syntax rules to [Github Linguist](https://github.com/github-linguist/linguist) so that J gets syntax highlighted on Github (in standalone as well as markdown files).
- `jconsole`:
   - add tldr page, man page, CLI `--help` & `--version`.
   - add README.md, CONTRIBUTING.md

## Install J Locally

### Install `jconsole` (terminal REPL) & JHS (web localhost interface)

See [platform-specific install instructions](https://code.jsoftware.com/wiki/System/Installation/J9.6).
For Fedora linux, do:

```console
$ wget https://www.jsoftware.com/download/j9.6/install/j9.6_linux64.tar.gz    # download archive
$ tar xvf j9.6_linux64.tar.gz     # extract archive
$ ./j9.6/bin/jconsole             # start terminal REPL
```

*Recommended* add `jconsole`, `jhs` to environment PATH by adding this in *~/.bashrc*
(assuming you installed it in your home):

```bash
export PATH="~/j9.6/bin:$PATH"
```

NOTE: This worked on Fedora 41, but after upgrading to Fedora 42 this issue is coming while trying to run `jconsole`:

```console
$ jconsole
Load library /home/sohangchopra/j9.6/bin/libj.so failed: /home/sohangchopra/j9.6/bin/libj.so: cannot enable executable stack as shared object requires: Invalid argument
```

**Terminal interface**:
Run `jconsole` (no argumets: start a terminal REPL) or `jconsole /path/to/script`. 
For more arguments, 
see [the official J wiki](https://code.jsoftware.com/wiki/System/Starting_J).

**Web (localhost) interface**:
In terminal run `jhs` - this starts a server on localhost and automatically opens browser at *http://127.0.0.1:65001/jijx*.
The web interace is basic (similar to terminal REPL) - type code in input, press Enter to get output.
JQt (the Qt-based IDE) is usually preferred, but if that doesn't work (eg. install error due to Qt dependencies),
then JHS is a useful fallback.

#### (Optional) [Run J shebang scripts](https://code.jsoftware.com/wiki/System/Installation/J9.6#J_scripting_support)

- Symlink (soft) to J interpreter: `sudo ln --symbolic /Applications/j9.6/bin/jconsole /usr/bin/ijconsole`.
- Write a test J script *script.j*:

```j
#! /usr/bin/ijconsole
echo 'Hello World'    NB must use single quotes only, double quotes don't work here
```

Making it executable `chmod +x script.j` and running it `./script` prints Hello World
as expected. Note that it doesn't end the script but instead leaves REPL running.


### (Optional) Install [`pacman` (J package manager)](https://code.jsoftware.com/wiki/Pacman) & [JQt IDE](https://code.jsoftware.com/wiki/Guides/Qt_IDE)

- JQt IDE requires Qt 5 - do `sudo dnf install qt5-qtwebengine-devel.x86_64`
(or `sudo dnf install qt5-devel` on older Fedora before that package was split),
otherwise `'install' jpkg '*'` (or `install 'full'`) line in next step complains:

```
jqt dependencies not found - jqt will not start until these are resolved
	libQt5WebEngineWidgets.so.5 => not found
```

NOTE 1: Found the required specific Qt package to install using 
`dnf list "qt5*devel" | grep web`.

NOTE 2: [JQt Ubuntu Install instructions](https://code.jsoftware.com/wiki/Guides/Qt_IDE/Install#Ubuntu.2FLinux_Mint)
lists some other Qt packages to install also, like `libqtwebsockets5`. 
I didn't install these but Fedora 41 didn't raise any issue when running JQt IDE - perhaps 
these Qt packages are already installed?
 
### (Optional) [Install / upgrade JQt IDE](https://code.jsoftware.com/wiki/Guides/Qt_IDE/Install#Upgrade)

Run `sh ~/j9.6/updatejqt.sh`(or equivalently construct path based on jconsole: 
`sh $(dirname $(which jconsole))/../updatejqt.sh`).

Alternatively run the following in `jconsole` REPL:

```j
   load'pacman'         NB load package manager
   'install' jpkg '*'   NB bring everything upto date
   
   NB alternative to above line (seems to do same thing?)
   NB Source: https://code.jsoftware.com/wiki/Guides/Qt_IDE/Install#Slim_vs_Full_Builds
   NB install 'full'
```

Now Open `jqt` (it's installed in same directory as jconsole: `dirname $(which jconsole)` - 
here *~/j9.6/bin/*) - it will launch JQt IDE.

Optionally [Configure JQt IDE](https://code.jsoftware.com/wiki/Guides/Qt_IDE/Configure).

NOTE: Check all usage ways of `jpkg` J function: 
https://code.jsoftware.com/wiki/Pacman#jpkg_Usage .

#### (Optional) Install JQt IDE Desktop Shortcut

[Official Docs](https://code.jsoftware.com/wiki/Pacman#Installing_Desktop_Shortcuts)
say to run the following in `jconsole`:

```j
  'shortcut' jpkg 'jqt'
  'shortcut' jpkg 'jhs'
  'shortcut' jpkg 'jc'
```

But this didn't seem to have any effect in my laptop (Fedora 41). 
So I manually installed desktop shortcut for JQt IDE:

- Download J logo: `curl https://code.jsoftware.com/mediawiki/resources/assets/jmedia.png?774cb --output j_language_logo.png`
- Add desktop entry so that Desktop Environment can find JQt IDE - 
  as root, create */usr/share/applications/jqt.desktop* 
  (modify Exec and Icon paths according to your system):

```desktop
[Desktop Entry]
Type=Application
Name=JQt IDE
Comment=JQt IDE for J programming language
# NOTE: quote around Exec but NOT around Icon is intentional -  
# due to quirks of .desktop format, 
# if we DON'T do this, the app icon often fails to show in Gnome Dash
Exec="/home/sohangchopra/j9.6/bin/jqt"                        
Icon=/home/sohangchopra/Pictures/j_language_logo.png   
MimeType=image/png               
Categories=Development
```

## Editor Integration

Besides the official JQt IDE mentioned above, you can also install extensions to support J in various editors.

### VS Code

- [VS Code J extension](https://marketplace.visualstudio.com/items/?itemName=tikkanz.language-j): adds syntax highlighting to J scripts.
- [Terminal Send extension (language-agnostic)](https://marketplace.visualstudio.com/items?itemName=sswatson.terminal-send) sends selected    text to VS Code integrated terminal. In VS Code integrated terminal, start `jconsole` REPL, then select code in a J script
  and use Shift+Enter to send & run selected code in `jconsole`.

## Usage

- Print to console: `echo 'Hello World'`
- Comments start with `NB.` (till end of the line): `1 2 + 3 4   NB. This is a comment.`. Mind the dot after NB !
- [Format / parse numbers](https://code.jsoftware.com/wiki/Guides/General_FAQ/Numbers_and_Character_Representations):

```j
   'r<0>4.0' 8!:0 i.5  NB. format numbers, padded with leading zeros
+----+----+----+----+----+
|0000|0001|0002|0003|0004|
+----+----+----+----+----+

   read =: 1!:1
   line =: read 1  NB. 1 for keyboard, can also put filename to read from file
1 2 3 4 5
   +/ ". line   NB. parse string to numbers array, then sum
10

   'Hello, ',(1!:1)1  NB. read name, print greeting
Sohang
Hello, Sohang
```
- [Rank conjunction `"`](https://www.jsoftware.com/help/learning/07.htm) can be used to apply a function on a particular axis. Similar to APL's Each `¨` for element wise operation.
- [This SO answer](https://stackoverflow.com/a/5087557/12947681) shows how to use [Power `^:`](http://www.jsoftware.com/help/dictionary/d202n.htm) to repeatedly apply a function to a value a number of times:

```j
NB. Apply function f 10 times, y is initial value.
f^:10 y
```

TODO:

- [Control Structures](https://www.jsoftware.com/help/dictionary/ctrl.htm) can only be used in Explicit functions (this page in J Primer gives good overview: https://www.jsoftware.com/help/jforc/control_structures.htm):
  - [`if.`](https://www.jsoftware.com/help/dictionary/cif.htm)
  - [`select.`](https://www.jsoftware.com/help/dictionary/csel.htm) - similar to `match` in Python.
  - `while.`, whilst.` (check J Primer page above)
  - [`for.`](https://www.jsoftware.com/help/dictionary/cfor.htm)
  - [`break.`](https://www.jsoftware.com/help/dictionary/cbreak.htm)
  - [`continue.`](https://www.jsoftware.com/help/dictionary/ccont.htm)
  - [`goto_name.`](https://www.jsoftware.com/help/dictionary/cgoto.htm)
  - [`return.`](https://www.jsoftware.com/help/dictionary/cret.htm)
  - [`throw.`](https://www.jsoftware.com/help/dictionary/cthrow.htm)
  - [`try.`](https://www.jsoftware.com/help/dictionary/ctry.htm)
  - [`assert.`](https://www.jsoftware.com/help/dictionary/cassert.htm)
- File I/O operations: https://code.jsoftware.com/wiki/Vocabulary/Foreigns#m1

## Real World Applications

- [Web Servers](https://code.jsoftware.com/wiki/Category:Web_R.14):
  - [Common Gateway Interface (CGI)](https://code.jsoftware.com/wiki/CGI)
  - [J Hypertext Processor (JHP)](https://code.jsoftware.com/wiki/JHP)
- [Jd](https://code.jsoftware.com/wiki/Jd/Overview) -
  proprietary column-oriented RDBMS database written in J.
  It's [partially open source](https://code.jsoftware.com/wiki/Jd/Overview#Open_source) -
  the J source code is available, though some proprietary binary blobs are not.

## Resources

- https://github.com/bugsbugsbux/understanding-j/
- Tutorial: [Absolutely Essential Terms](https://code.jsoftware.com/wiki/Vocabulary/AET)
- [Minimal Subset of J](https://code.jsoftware.com/wiki/User:Devon_McCormick/MinimalBeginningJ)
- J Vocablary: 
  [NuVoc: the Wiki dictionary of J](https://code.jsoftware.com/wiki/NuVoc)
- [J Wiki (online version)](https://code.jsoftware.com/wiki/)
- [J Reference](https://code.jsoftware.com/wiki/Category:Reference_R)
- [J for the APL Programmer](https://code.jsoftware.com/wiki/Doc/J4APL#J_for_the_APL_Programmer) - 
  J equivalents to APL functions are shown in 
  [APL -> J Phrase Book](https://code.jsoftware.com/wiki/APL2JPhraseBook).
- [J Shell Scripting Guide](https://code.jsoftware.com/wiki/Guides/J_Shell_Script)
- [J Essays](https://code.jsoftware.com/wiki/Essays)

### Exercises: [J Showcase](https://code.jsoftware.com/wiki/Showcase)

- [J Graphics Gallery](https://code.jsoftware.com/wiki/Studio)
- [J Puzzles](https://code.jsoftware.com/wiki/Puzzles)
- Practical annotated programming tasks: 
  [Share My Screen](https://code.jsoftware.com/wiki/ShareMyScreen)

### [Videos](https://code.jsoftware.com/wiki/VideoInstructionInJ)

YouTube videos on J: a lot of the listed videos are by 
[@bobtherriault](https://www.youtube.com/@bobtherriault) and 
[@tangentstorm](https://www.youtube.com/@tangentstorm).

### [Blogs](https://code.jsoftware.com/wiki/Category:Blogs_C.2)

- [Stevan Apter (No Stinking Loops)](https://nsl.com/)
- [John Earnest (Decker, Lil, k)](https://beyondloom.com/blog/)
- [Scott Locklin Blog](https://scottlocklin.wordpress.com/category/tools/j/)
- [Andrew Nikitin Scripts](http://nsg.upor.net/jpage/jpage.htm)

### Books

- [J Primer](http://www.jsoftware.com/help/primer/contents.htm)
- [J System Documentation](https://code.jsoftware.com/wiki/System/Documentation)

[Some more books on J](https://code.jsoftware.com/wiki/Books):
- [Learning J](https://www.jsoftware.com/help/learning/contents.htm)
- [J for C Programmers (PDF)](https://code.jsoftware.com/mediawiki/images/8/80/JforC20071003.pdf)
- [Easy J (zip archive containing PDF)](http://www.jsoftware.com/books/pdf/easyj.zip)
- Visual Cheat Sheet (2 page summary): 
  [J Reference Card (PDF)](https://code.jsoftware.com/mediawiki/images/5/53/J602_RefCard_color_letter_current.pdf)

### Tooling

- [J Graphic Analyzer](https://code.jsoftware.com/wiki/Guides/Getting_Started#J_Graphic_Analyzer) -
  A single line of J does a whole lot; 
  [Dissect](https://code.jsoftware.com/wiki/Vocabulary/Dissect#Dissect) helps in debugging
  by converting program flow to pictorial format 
  (at least it's supposed to, but running this code didn't launch any image:

```j
   require 'debug/dissect'
   dissect '1 2 + 3 4 * 5 6'
value error
|value error: wd
|       wd DISSECT

|value error: wd
|       wd'timer 0'
Press ENTER to inspect
   
dbr 0 to end inspection; use y___1 to look inside top stack frame (see code.jsoftware.com/wiki/Debug/Stack#irefs)
```
- [ArrayPortal](https://www.arrayportal.com/) - supports all array oriented languages 
  (APL and derivatives). In J, it searches resources like J Wiki and J Forums.

### [Communities](https://code.jsoftware.com/wiki/Category:Community_C)

- [J Forum (Google Group mailing list)](https://code.jsoftware.com/wiki/System/Forum) - 
  email forum-subscribe@jsoftware.com with any body (it's ignored) and subject one of:
   - "all": get all emails
   - "digest": max one daily email
   - "nomail": dont get any mail, but can still send email to mailing list
- [J on Stack Overflow](https://stackoverflow.com/questions/tagged/j)
- Subreddit for all array oriented languages (APL and derivatives, including J): 
  [r/apljk](https://reddit.com/r/apljk).

#### [Official Contact Info](https://www.jsoftware.com/#/contact)

Emails to directly contact JSoftware, the company behind J:

- info@jsoftware.com - licensing/training/consulting/general enquiries
- dev@jsoftware.com - J engine development group
- jal@jsoftware.com - application library issues
- qt@jsoftware.com - Qt IDE issues
- wiki@jsoftware.com - wiki-related issues
