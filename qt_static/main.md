
# How to build Static Qt on Windows

**Note**: I am going to build the 32 bits version of MinGW so that the final program will work on both 32 bit and 64 bit machines, you can use the 64 bits version if you only need your final program to work on 64 bit machines.
*Everywhere you see the word `32-bit` replace it with `64-bit` for the 64 bit version of Static Qt.*

---

# Downloading
Download the [Open Source Qt Online Installer](https://www.qt.io/download).

Once in the **Select Components** window, select **MinGW** along with **Sources**.
This is what the window should look like:

<img src="https://raw.githubusercontent.com/TheGameratorT/TheGameratorT.github.io/master/qt_static/pkg_select.png" alt="Select Components window"/>

# Building
Proceed with the installation and after everything has finished open `Qt 5.15.2 (MinGW 8.1.0 32-bit)` from the Windows search bar.

Now type the following with the templates changed:
```cmd
cd <qt_path>\<qt_version>\Src
configure.bat -prefix "<qt_path>\Static\<qt_version>\<env_name>" -release -opensource -confirm-license -static -static-runtime -opengl desktop -skip qtwebengine -nomake examples -nomake tests
mingw32-make -j<thread_count>
mingw32-make -j<thread_count> install
```

| Template         | Description                           | Example                      |
|------------------|---------------------------------------|------------------------------|
| `<qt_path>`      | The path to Qt                        | `C:\Qt`                      |
| `<qt_version>`   | The version of Qt                     | `5.15.2`                     |
| `<env_name>`     | The name of the environment           | `mingw81_32` or `mingw81_64` |
| `<thread_count>` | The number of threads of the computer | `8`                          |

**Note**: If you need to use the Qt WebEngine module remove `-skip qtwebengine` from the template, I skip it by default because it is huge and not used in most of the cases.

---

Example:
```cmd
cd C:\Qt\5.15.2\Src
configure.bat -prefix "C:\Qt\Static\5.15.2\mingw81_32" -release -opensource -confirm-license -static -static-runtime -opengl desktop -skip qtwebengine -nomake examples -nomake tests
mingw32-make -j8
mingw32-make -j8 install
```

# Adding to Qt Creator
Open Qt Creator and in the menu bar open **Tools** -> **Options** and selects **Kits**.
In the **Qt Versions** sub-tab add a new entry and set:
 - Version name: `Qt Static %{Qt:Version} MinGW 32-bit`
 - qmake location: `<qt_path>\Static\<qt_version>\<env_name>\bin\qmake.exe`

Now in the **Kits** sub-tab duplicate `Desktop Qt %{Qt:Version} MinGW 32-bit` and set:
 - Name: `Desktop Qt Static %{Qt:Version} MinGW 32-bit`
 - Qt version: `Qt Static %{Qt:Version} MinGW 32-bit`

After all this, Static Qt should be ready to use.
