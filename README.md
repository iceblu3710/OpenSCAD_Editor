# OpenSCAD Editor

A sample OpenSCAD editor with the basic features of a code editor, like syntax highlighting and autocompletion.


# Features

* Available on Linux, Mac OSX and Windows
* Syntax highlighting
* Code folding
* Find & Replace
* Autocomplete

NOTE:
  Currently I do not have autocomplete working and the syntax highlighting is kind of wonky. But the editor does work.

**This editor requires node-webkit >= v0.3.0** 

## Screen shot

![screenshot](https://raw.github.com/iceblu3710/OpenSCAD_Editor/master/img/editor.jpg)
![screenshot](https://raw.github.com/iceblu3710/OpenSCAD_Editor/master/img/example.jpg)

# Quick Start

* Download node-webkit from one of the links below:
Prebuilt binaries (v0.4.0 — Jan 18, 2013):
Linux: [32bit](https://s3.amazonaws.com/node-webkit/v0.4.0/node-webkit-v0.4.0-linux-ia32.tar.gz) / [64bit](https://s3.amazonaws.com/node-webkit/v0.4.0/node-webkit-v0.4.0-linux-x64.tar.gz)
Windows: [win32](https://s3.amazonaws.com/node-webkit/v0.4.0/node-webkit-v0.4.0-win-ia32.zip)
Mac: [32bit, 10.7+](https://s3.amazonaws.com/node-webkit/v0.4.0/node-webkit-v0.4.0-osx-ia32.zip)

* Open/Install OpenSCAD and in the menu select `View -> Hide Editor` and `Design -> Automatic Reload and Compile`

* unzip the node-webkit binary and cut/copy the `nw.exe` and `nw.pak` files into the OpenSCAD editor folder

* Run `nw.exe` and open a .scad file in both the editor AND in OpenSCAD itself

* Whenever you make changes in the editor press `F5` or `Ctrl-S` and OpenSCAD will render the changes

# License

`node-webkit`'s code uses the MIT license, see their [`LICENSE`](https://github.com/rogerwang/node-webkit/blob/master/LICENSE) file.
