# How to install GTK+3 and GTKMM on Windows with CMake

Follow the steps in [https://www.msys2.org/](https://www.msys2.org/) to install MSYS2.

After MSYS2 is installed add the following to PATH as the first entry:
`<path to msys64>\mingw64\bin`

Now open MSYS2 from it's directory and run:
```
pacman -S mingw-w64-x86_64-pkg-config
pacman -S mingw-w64-x86_64-cmake
pacman -S mingw-w64-x86_64-gcc
pacman -S mingw-w64-x86_64-gtk3
pacman -S mingw-w64-x86_64-gtkmm3
```
Your MSYS2 environment should now be ready for compilation.

Proceed to create a CMake project with the following `CMakeLists.txt` as reference:

```cmake
# CMakeLists.txt : CMake project for CMakeTest, include source and define project specific logic here.
cmake_minimum_required (VERSION 3.8)

project("CMakeTestGTK")

# Use the package PkgConfig to detect headers/library files
find_package(PkgConfig REQUIRED)
pkg_check_modules(GTK3 REQUIRED gtk+-3.0)
pkg_check_modules(GTKMM REQUIRED gtkmm-3.0)

# Tell the compiler where to look for headers and to the linker where to look for libraries
include_directories(${GTK3_INCLUDE_DIRS} ${GTKMM_INCLUDE_DIRS})
link_directories(${GTK3_LIBRARY_DIRS} ${GTKMM_LIBRARY_DIRS})

# Add other flags to the compiler
add_definitions(${GTK3_CFLAGS_OTHER})

# Add source to this project's executable.
file(GLOB_RECURSE SOURCES RELATIVE ${CMAKE_SOURCE_DIR} "source/*.cpp")
add_executable (${PROJECT_NAME} WIN32 ${SOURCES})

target_link_libraries(${PROJECT_NAME} ${GTK3_LIBRARIES} ${GTKMM_LIBRARIES})
```

If you are using Visual Studio you can use the following CMakeSettings.json as reference:
Note: You might need to change the WindowsSDK paths in INCLUDE and the path to mingw64 in MINGW64_ROOT.
```json
{
  "configurations": [
    {
      "environments": [
        {
          "MINGW64_ROOT": "<path to msys64>\\mingw64",
          "BIN_ROOT": "${env.MINGW64_ROOT}\\bin",
          "FLAVOR": "x86_64-w64-mingw32",
          "TOOLSET_VERSION": "10.2.0",
          "PATH": "${env.MINGW64_ROOT}\\bin;${env.MINGW64_ROOT}\\..\\usr\\local\\bin;${env.MINGW64_ROOT}\\..\\usr\\bin;${env.MINGW64_ROOT}\\..\\bin;${env.PATH}",
          "INCLUDE": "${env.MINGW64_ROOT}\\include;C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\VC\\Tools\\MSVC\\14.27.29110\\include;;C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\VC\\Tools\\MSVC\\14.27.29110\\atlmfc\\include;C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\VC\\Auxiliary\\VS\\include;C:\\Program Files (x86)\\Windows Kits\\10\\Include\\10.0.17763.0\\ucrt;C:\\Program Files (x86)\\Windows Kits\\10\\Include\\10.0.17763.0\\um;C:\\Program Files (x86)\\Windows Kits\\10\\Include\\10.0.17763.0\\shared;C:\\Program Files (x86)\\Windows Kits\\10\\Include\\10.0.17763.0\\winrt;C:\\Program Files (x86)\\Windows Kits\\10\\Include\\10.0.17763.0\\cppwinrt;C:\\Program Files (x86)\\Windows Kits\\NETFXSDK\\4.8\\Include\\um",
          "environment": "mingw_64"
        }
      ],
      "name": "Mingw64-Debug",
      "generator": "Ninja",
      "configurationType": "Debug",
      "inheritEnvironments": [ "mingw_64" ],
      "buildRoot": "${projectDir}\\out\\build\\${name}",
      "installRoot": "${projectDir}\\out\\install\\${name}",
      "cmakeCommandArgs": "",
      "buildCommandArgs": "-v",
      "ctestCommandArgs": "",
      "intelliSenseMode": "windows-clang-x64",
      "variables": [
        {
          "name": "CMAKE_C_COMPILER",
          "value": "gcc.exe",
          "type": "FILEPATH"
        },
        {
          "name": "CMAKE_CXX_COMPILER",
          "value": "g++.exe",
          "type": "FILEPATH"
        }
      ]
    }
  ]
}
```
The project structure should look something like this:
```
project
│   CMakeLists.txt
│   CMakeSettings.json (if you use Visual Studio)
|
└───source
    │   main.cpp
```
Compile using the console:
- Open the console in the project directory.
- Run `cmake .`
- Run `make`
- You should see your newly built .exe

Compile using Visual Studio:
 - Open the project directory as folder.
 - Click the build or start button.

Deploying the application:
Navigate to `<path to msys64>\mingw64\bin` and copy the required DLLs to the executable directory.
