[![Linux](https://github.com/nCine/ncTemplate/workflows/Linux/badge.svg)](https://github.com/nCine/ncTemplate/actions?workflow=Linux)
[![macOS](https://github.com/nCine/ncTemplate/workflows/macOS/badge.svg)](https://github.com/nCine/ncTemplate/actions?workflow=macOS)
[![Windows](https://github.com/nCine/ncTemplate/workflows/Windows/badge.svg)](https://github.com/nCine/ncTemplate/actions?workflow=Windows)
[![MinGW](https://github.com/nCine/ncTemplate/workflows/MinGW/badge.svg)](https://github.com/nCine/ncTemplate/actions?workflow=MinGW)
[![Emscripten](https://github.com/nCine/ncTemplate/workflows/Emscripten/badge.svg)](https://github.com/nCine/ncTemplate/actions?workflow=Emscripten)
[![Android](https://github.com/nCine/ncTemplate/workflows/Android/badge.svg)](https://github.com/nCine/ncTemplate/actions?workflow=Android)
[![CodeQL](https://github.com/nCine/ncTemplate/workflows/CodeQL/badge.svg)](https://github.com/nCine/ncTemplate/actions?workflow=CodeQL)

# ncTemplate
A template project for applications and games made with the nCine.

For most projects the amount of customization offered by this script system will be enough.
They will be able to target all engine supported platforms: Linux, macOS, Windows, MinGW, Android and Emscripten.

## Customization

### Project information

The only CMake script you need to modify for your project is `CMakeLists.txt`.

It exposes a number of different variables that will be used by the template script system distributed with the engine:

- **NCPROJECT_NAME**: The name of your project
- **NCPROJECT_EXE_NAME**: The name of the executable file for your project (without any extension).
  If you don't set it the default will be a lower case version of `${NCPROJECT_NAME}`.
- **NCPROJECT_VENDOR**: You can set it to your company name or your personal name (used by installers).
- **NCPROJECT_COPYRIGHT**: The copyright dates and notes for the project.
- **NCPROJECT_DESCRIPTION**: A short description of your project (used by installers).
- **NCPROJECT_HOMEPAGE**: The URL of your project homepage (used by installers).
- **NCPROJECT_REVERSE_DNS**: The name of your project expressed in reverse domain name notation.
  It is used for the XDG Desktop file on Linux, the Bundle Identifier on macOS and for the java package namespace on Android.
- **NCPROJECT_XDGDESKTOP_CATEGORIES**: The application [categories](https://specifications.freedesktop.org/menu-spec/latest/apa.html) for the XDG Desktop file on Linux.
- **NCPROJECT_MACOS_CATEGORY**: The application [category](https://developer.apple.com/documentation/bundleresources/information_property_list/lsapplicationcategorytype) for the Info.plist file on macOS.

- **NCPROJECT_INCLUDE_DIRS**: If your project uses one or more separate include directories for headers you can specify them with this variable.
  The paths to the include directories can be absolute or relative to `${CMAKE_SOURCE_DIR}.`
- **NCPROJECT_SOURCES**: The list of source files that will be compiled.
  The paths to the source files can be absolute or relative to `${CMAKE_SOURCE_DIR}.`
- **NCPROJECT_ANDROID_ASSETS**: A list of files that will become Android assets and part of your project APK archive.
  The paths to the asset files can be absolute or relative to `${NCPROJECT_DATA_DIR}`.

This file also supports the declaration of some callback functions that will be called be the main template script:
- **callback_start()**: This function will be called at the very beginning, just after calling the CMake `project()` command.
- **callback_before_target()**: This function will be called just before declaring the executable target for your project.  
  It will happen after having set and checked all project information, found the engine and retrieved the project version.
- **callback_after_target()**: This function will be called later on, when your project executable target `${NCPROJECT_EXE_NAME}` is already declared.
  At this time most of the target properties are already set, like sources, include directories, compile definitions and compile options.
- **callback_end()**: This function will be called at the very end, when all the variables are set and all the external scripts have been included.

### Project options

There are further options that you can setup when invoking CMake:

- **NCPROJECT_BUILD_ANDROID**: When enabled the scripts will try to build an Android version of the project.
  When this option is enabled you have access to an additional set of variables:
  - **NDK_DIR**: The path to the Android NDK directory.  
    You need to set this variable if the script system can detect any NDK related environment variable in your system.
  - **NCPROJECT_NDK_ARCHITECTURES**: A list of target CPU architectures for the Android version of your project.
    The default is `arm64-v8a`.
  - **NCPROJECT_ASSEMBLE_APK**: If the option is on then the CMake script will try to assemble an APK by invoking Gradle.
  - **NCPROJECT_UNIVERSAL_APK**: If the option is on the Android Gradle script is configured to assemble an additional APK that contains all ABIs specified in `${NCPROJECT_NDK_ARCHITECTURES}`.
- **NCPROJECT_STRIP_BINARIES**: When turned on binary files will be stripped of their symbols to reduce their size.
  It does not work on Windows.
- **NCPROJECT_DATA_DIR**: The path to the project root data directory.
  The default is to search for a directory at the same level of your project one that has a `-data` suffix.  
  You don't usually need to change the value of this variable.
- **NCINE_EXTERNAL_DIR**: The path to the nCine external dependencies directory.  
  The default is to search for a directory at the same level of your project one that is called `nCine-external`.  
  You don't usually need to change the value of this variable.
- **NCPROJECT_WITH_CRASHPAD**: When enabled, the `crashpad_handler` will be copied from the nCine directory (if available) to the executable directory.
- **NCPROJECT_DEBUGINFO**: If this variable is set to `EXTRACT`, debug symbols will be extracted after building the executable and put in a separate file.
  If it is set to `INSTALL`, then the symbols will be extracted and installed along with the game package.
- **DEBUGINFO_DIR**: The path where the extracted symbols will be placed. The default value is a `symbols` directory inside the build directory.
- **CMAKE_SKIP_INSTALL_RPATH**: Set this to `TRUE` when preparing a Linux or MinGW distribution package. CMake will avoid adding `RUNPATH` entries to installed binaries (on Linux), and the nCine project scripts will skip copying dependency libraries during installation.

### Android-only variables

On Android you have access to two more special variables:

- **NCPROJECT_SOURCE_DIR**: This is equal to the original non-android `${CMAKE_SOURCE_DIR}` variable.
- **NCPROJECT_BINARY_DIR**: This is equal to the original non-android `${CMAKE_BINARY_DIR}` variable.

### Additional files

- Don't forget to rewrite the `LICENSE` and `README.md` files with the information related to your project.
- Don't forget to read the `README.md` file in the `ncTemplate-data` repository for more customization information.

## Notes

The template scripts will add two compiler definitions when the project is compiled in `Debug` mode.
One is `NCPROJECT_DEBUG`, while the other is `${NCPROJECT_UPPER_NAME}_DEBUG` which, for this specific project, will become `NCTEMPLATE_DEBUG`.
