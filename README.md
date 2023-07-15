# QMLIntegrationQt6
About Qt, QtCreator, QML, QtQuick, CMake and Ninja, with a Qt6 project that demonstrates how to connect QML to a C++ Class.  Some hints about porting a Qt5 Project to Qt6.  I hope you find it useful :-)  Howerd Oakford www.inventio.co.uk

README.TXT  Qt6, QML and CMake   2023 Jul 15

Links
-----
Qt6         https://www.qt.io/
QML         https://www.qt.io/product/qt6/qml-book
QtCreator   https://www.qt.io/product/development-tools
CMake       https://cmake.org/
Ninja       https://ninja-build.org/

How to port a Qt5 QtWidgets application to Qt6_QML - an overview
================================================================

Qt5 uses qMake, Qt6 uses CMake
------------------------------

In the DOS era (~1980) there was make.exe, a program that was stored in the filesystem in
the place where other program tools (compilers, editors) are kept.
The system-wide environment variable $PATH was extended when make.exe was installed, to include make.exe. 
When a terminal (DOS cmd shell) is opened in the directory where your source files are stored,
typing  'make'  executes make.exe, which looks for a file called 'makefile'.
So 'makefile' contains the targets and instructions for building your program.

Qt5 uses YourProgram.pro and YourProgram.pri files to specify the targets and instructions for building your application.

qMake to CMake
There is a Python3 project to create CMakeLists.Txt files from *.pro and *.pri files :
https://pypi.org/project/qmake2cmake/ - this may help.

Qt6 uses files called CMakeLists.Txt, located in folders and sub-folders in your source code tree.
The top level CMakeLists.Txt file defines the location of other CMakeLists.Txt files in your source sub-folders.
In some ways this is going back to the DOS make system, where there is only one filename, used in different folders.

CMake uses a Meta Object Compiler ("moc") to create files for Ninja. Ninja is a program that converts these files
into makfiles in the correct format for the different possible targets (Windows, MacOS, Linux etc.).

Version Control
===============
CMakeLists.Txt files  import libraries, with an optional version number for example in Main.qml :
    import QMLIntegrationQt6 1.0
imports version 1.0 of the library we are creating, the file CMakeLists.Txt specifies the version we are creating :
qt_add_qml_module(appQMLIntegrationQt6
    URI QMLIntegrationQt6
    VERSION 1.0
    QML_FILES Main.qml
    randcalc.h
    randcalc.cpp
)
These versions must match, or the correct (versioned) library will not be found.

Making a New Qt6_QML project
----------------------------

Qt Creator makes it easy to create a new project - select Home, New Project, QtQuick (which uses QML).
The Main.qml file that is created specifies versions for all Qt6 libraries.
Qt6.5 defaults to version 6.4 for some reason.
If you export your project, these version numbers may not match the current versions on another system.
To solve this problem, the other system could create a new QtQuick (QML) project to find out what the
versions should be for this system. Or you can simply delete the version numbers...

Because of the complexity of the CMake system, simple errors in a CMakeLists.txt file can produce 
cryptic error messages : "library not found", "ninja error" etc.
The best way to cope with this is to start with a working system and make incremental changes.

Qt6_QML Connecting to C++ Classes 
---------------------------------
This YouTube video for example shows the process of creating a new QtQuick project and adding
signals and slots that allow QML and C++ classes to exchange information - clicking a QML button
runs some C++ code that then updates the QML display.

https://www.youtube.com/watch?v=knlH_NF8PAs  (2022 Nov 08)

There are various Qt6 keywords that need to be absolutely correct to avoid cryptic errors : 
Failing to add QML_ELEMENT in the C++ Class definition will cause the Class methods not to be found.
A typo such as CMAKE_AUTO_MOC instead of CMAKE_AUTOMOC gives a "ninja error".
But once you have got these things right life becomes simpler...

The Qt5_QML Project zipped in the file "QMLIntegrationQt6.7z" is a transcription of the code from the video,
with one spelling correction (integration instead of inteRgration - sorry for being a bit OCD ;-)

I hope that this document and the QMLIntegrationQt6 Project are a good starting point for understanding Qt6, QML and CMake.

Happy Hacking!

Howerd Oakford  www.inventio.co.uk

