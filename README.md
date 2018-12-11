# Tangent Animation's Blender Made Buildable
Thanks to the guys at Tangent Animation, we now have a Blender fork that has nice features like **Alembic** and **OpenVDB volumes** support and plenty of other stuff and optimizations a real animation studio has implemented for its real production work.

Personally I was dying to get OpenVDB smoke simulation volumes import from Houdini so I've managed to build that fork for myself and now sharing the very minor fixes I had to do to make this thing building from scratch and working.

Instructions below are based on original Blender's wiki text with a few changes related specifically to this fork.


# Building on 64-bit Windows

These are instructions to build Blender for Windows, with Microsoft Visual Studio. We officially support building with Visual Studio version 2015 and 2017, older versions do not work.

## Quick Setup

Building Blender is not as hard as most people would think. For beginners, easiest is to follow these steps carefully. For more details and alternative ways to set up a build environment, see below.

### Install Development Tools

Subversion, Git, CMake and Visual Studio ***must all be installed***.

* Install [Visual Studio 2017 Community Edition](https://visualstudio.microsoft.com) (free)
* Install [Subversion for Windows](http://www.sliksvn.com/en/download) (SlikSVN)
* Install [Git for Windows](https://gitforwindows.org/)
  * In the installer, choose to '''add Git to your PATH''' to ensure the Git version is in the splash screen.
* Install [CMake](http://cmake.org)
  * In the installer set the system path option to '''Add CMake to the system PATH for all users'''.
* Optional for NVIDIA GPUs: install [CUDA 9.1](https://developer.nvidia.com/cuda-downloads) (exact version, newer versions may not work) for CUDA support in Cycles.

### Download Sources and Libraries

Create a folder to store your copy of the source code and libraries. This guide will assume your chosen folder is `C:\blender-git`.

Then open the command prompt window by hitting Windows+R, and then typing cmd, or by searching for it in the start menu. Then type the following commands.

Check out the precompiled libraries with Subversion. *Note the older revision 62019 being checked out:*
```
cd C:\blender-git
svn checkout -r62019 https://svn.blender.org/svnroot/bf-blender/trunk/lib/win64_vc14  lib/win64_vc14
```
   
Download [embree-2.16.5.x64.windows.zip](https://github.com/embree/embree/releases/download/v2.16.5/embree-2.16.5.x64.windows.zip). Unzip it to `C:\blender-git`. Path-wise you are expected to get `C:\blender-git\embree-2.16.5.x64.windows\embree-config.cmake`.

Download this Blender fork's source code:
```
cd C:\blender-git
git clone git@github.com:tarasvb/blender278.git
git checkout build-patches
cd blender
git submodule update --init --recursive
```

### Compile Blender

```
cd C:\blender-git\blender
make full
```

Once the build finishes you'll get a message like this, pointing to the freshly built Blender:
``` 
Blender successfully built, run from: C:\blender-git\build_windows_Full_x64_vc14_Release\bin\Release
```

### Copy Embree DLL files

This Blender build will need `embree.dll` and `tbb.dll` in order to run.
You may find them at `C:\blender-git\embree-2.16.5.x64.windows\bin\`.
Simply copy these to the same folder your `blender.exe` resides.
