# How to build Static Qt on Windows

Download Open Source Qt Online Installer at [https://www.qt.io/download](https://www.qt.io/download).

Go to [https://download.qt.io/archive/qt/](https://download.qt.io/archive/qt/) and search for the latest `qt-everywhere-src-X.XX.X.zip` file.

```bat
configure.bat -prefix "D:\Qt\Static\5.15.1\mingw81_32" -release -opensource -confirm-license -static -static-runtime -opengl desktop -skip qtwebengine -nomake examples -nomake tests
mingw32-make -j8
mingw32-make -j8 install
```