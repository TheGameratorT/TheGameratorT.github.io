# How to build Static Qt on Windows

Download the [Open Source Qt Online Installer](https://www.qt.io/download).

Once in the **Select Components** window, select **MinGW** along with **Sources**.
_**Note**: I am going with the 32 bits version of MinGW so that the end result will work on both 32 bit and 64 bit machines, you can use the 64 bits version if you only need your final program to work on 64 bit machines. If you want both, then you must compile static Qt twice one on each compiler._

This is what the **Select Components** window should look like:
<img src="https://raw.githubusercontent.com/TheGameratorT/TheGameratorT.github.io/master/qt_static/pkg_select.png" alt="Select Components window"/>

```bat
configure.bat -prefix "D:\Qt\Static\5.15.1\mingw81_32" -release -opensource -confirm-license -static -static-runtime -opengl desktop -skip qtwebengine -nomake examples -nomake tests
mingw32-make -j8
mingw32-make -j8 install
```