# Description

This directory contains the files needed to use the [PCRE library](https://www.pcre.org/) with
the native Windows C++ API.

Source: https://github.com/PCRE2Project/pcre2/releases

The sources have been compiled using Microsoft Visual C++ (from [Visual Studio](https://visualstudio.microsoft.com/fr/vs/features/cplusplus/) 2022).

# Build instructions

## Prerequisite

* CMAKE: https://cmake.org
* MSVC: https://visualstudio.microsoft.com/fr/vs/features/cplusplus/
* PCRE: https://github.com/PCRE2Project/pcre2/releases

CMAKE version:

    PS C:\pcre2-pcre2-10.44> & "C:\Program Files\CMake\bin\cmake" --version
    cmake version 3.30.0-rc1

> By default, CMAKE is installed under `C:\Program Files\CMake\bin`.
>
> Command line to start the cmake GUI: `& "C:\Program Files\CMake\bin\cmake-gui"`

MSVC (_community edition - 2022_) compiler version:

    PS C:\pcre2-pcre2-10.44> cl /?
    Compilateur d'optimisation Microsoft (R) C/C++ version 19.40.33811 pour x64

> By default, MSVC is installed under `C:\Program Files\Microsoft Visual Studio`.

PCRE version: `2-10.44`

> For example (the ZIP archive): https://github.com/PCRE2Project/pcre2/archive/refs/tags/pcre2-10.44.zip

## Configure the terminal environment

Find the path to the MSVC executable tools:

    Get-Childitem –Path 'C:\Program Files\Microsoft Visual Studio' -Include cmake.exe -File -Recurse -ErrorAction SilentlyContinue | Select-Object FullName | Select-String -Pattern "Hostx64\\x64" -AllMatches

Path: `C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.40.33807\bin\Hostx64\x64`

Find the path to MSBUILD:

    Get-Childitem -Path 'C:\Program Files\Microsoft Visual Studio' -Include msbuild.exe -File -Recurse -ErrorAction SilentlyContinue | Select-Object FullName

Path: `C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\MSBuild.exe`

Find the path to CMAKE:

    Get-Childitem -Path 'C:\Program Files' -Include cmake.exe -File -Recurse -ErrorAction SilentlyContinue | Select-Object FullName

Path: `C:\Program Files\CMake\bin\cmake.exe`

> If you find more than one instance of CMAKE, you must use the one that you installed from the [CMAKE web site](https://cmake.org).

Configure the PowerShell terminal environment:

    $env:path += ";C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.40.33807\bin\Hostx64\x64"
    $env:path += ";C:\Program Files\CMake\bin"
    $env:path += ";C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin"

## Build instructions

Unzip the archive that contains the PCRE library sources.

Change directory to the extracted source code: `pcre2-10.44`

Run CMAKE:

    & "C:\Program Files\CMake\bin\cmake-gui"

Select the path to the source code. For example: `C:/Users/denis.beurive/Documents/dev/pcre2-pcre2-10.44`

Set the path to the build directory. For example: `C:/Users/denis.beurive/Documents/dev/pcre2-pcre2-10.44/build`

Press "`Configure`" and then "`Generate`".

Build the library:

    msbuild ALL_BUILD.vcxproj

Find the generated library:

    Get-Childitem -Path '.' -Include *.lib -File -Recurse -ErrorAction SilentlyContinue | Select-Object FullName

Result:

* `build\Debug\pcre2-8-staticd.lib`
* `build\Debug\pcre2-posix-staticd.lib`

Find the header files:

    Get-Childitem -Path '.' -Include *.h -File -Recurse -ErrorAction SilentlyContinue | Select-Object FullName

Result:

* `C:\Users\denis.beurive\Documents\dev\pcre2-pcre2-10.44\build\config.h`
* `C:\Users\denis.beurive\Documents\dev\pcre2-pcre2-10.44\build\pcre2.h`

> The header file which we need is `pcre2.h`.
