# Easily build boost on Visual Studio 2017
Easily build the boost library on last VS release, no pain

## build_boost_1_63_0_vs2017_win32_x64.bat

### Description
Windows batch script to build the boost library.
Builds boost with all runtime link types for both 32 and 64 bit architectures.

### Versions
- Tested with boost 1.63.0 (build_boost_1_63_0_vs2017_win32_x64.bat)
- Tested with 1.64.0 beta 2 (build_boost_1_64_0_b2_vs2017_win32_x64.bat)

### Instructions
1. Copy the .bat file inside boost's directory (eg C:\boost_1_63_0).

2. **Using Visual Studio 2017's Developer Command Prompt** (found in "Windows' Start"\All Programs\Visual Studio 2017\Visual Studio Tools), go to the boost root directory (eg. C:\boost_1_63_0) and call-execute to bat provided in this repo build_boost_1_63_0_vs2017_win32_x64.bat.

Note: Go for 2 Coffees... the process takes a long time.
It will output the .DLLs and .LIBs up one level from the current path (eg. d:\sdk\bin...) at "bin\x86" (for 32 bit) and "bin\x64" (for 64 bit).

## build_boost_1_64_0_b2_vs2017_win32_x64.bat
### Instructions for 1.64.0 boost beta 2 & Visual Studio 2017

```shell
cd A:\sources\libs\boost_1_64_0
build_boost_1_64_0_b2_vs2017_win32_x64.bat
```

### Next Steps - Linking Boost With Visual Studio 2017
1. Add headers
	- Go to Solution Explorer -> Properties -> C/C++ -> General
	- Pick & Set on "Additional Include Directories" = BOOST_ROOT (eg. C:\boost_1_64_0)

	- Properties -> C/C++ -> General -> Precompiled Header
	- Pick & Set on "Precompiled Header" to option Not Using Precompiled Headers

2. Add compiled libraries
	- Go to Solution Explorer -> Properties -> Linker -> General
	- Pick & Set on "Additional Library Directories" = BOOST_ROOT\lib (eg. C:\boost_1_64_0\stage_x86\lib)


# Method 2
1. bootstrap
2. b2 toolset=msvc-14.1 threading=multi link=static runtime-link=shared runtime-link=static variant=release variant=debug


vs2019

1. START > Visual Studio 2019 > x64 Native Tools Command Prompt 
2. cd  D:\boost.win 
3. .\bootstrap.bat 
4. Edit generated project-config.jam and replace 

using msvc ; 
with 
using msvc : 14.1 : "C:\\Program Files (x86)\\Microsoft Visual 
Studio\\2019\\Preview\\VC\\Tools\\MSVC\\14.20.27323\\bin\\HostX64\\x64\\cl.exe" 
; 

5. Before you fire b2, you can remove -nologo from this line in 
D:\boost.win\tools\build\src\tools\msvc.jam 

https://github.com/boostorg/build/blob/20d72776c8b61613f0e3b32d01b17f9ee013db0d/src/tools/msvc.jam#L1488

6. Finally, run b2 

.\b2 variant=debug address-model=64 --with-filesystem --with-test 
--layout=system 

... 
compile-c-c++ bin.v2\libs\filesystem\build\msvc-14.1\debug\address-model-64\link-static\threading-multi\codecvt_error_category.obj 
Microsoft (R) C/C++ Optimizing Compiler Version 19.20.27323 for x64 
Copyright (C) Microsoft Corporation.  All rights reserved. 

cl "libs\filesystem\src\codecvt_error_category.cpp" 
-Fo"bin.v2\libs\filesystem\build\msvc-14.1\debug\address-model-64\link-static\threading-multi\codecvt_error_category.obj" 
   -TP /Z7 /Od /Ob0 /W3 /GR /MDd /Zc:forScope /Zc:wchar_t /favor:blend 
/wd4675 /EHs -c 
   -DBOOST_ALL_NO_LIB=1 
   -DBOOST_FILESYSTEM_STATIC_LINK=1 
   "-I." 

and you should see the CL 19.20.27323 from VS2019 is actually used, that is: 


D:\boost.win>cl /? | findstr optimized 
Microsoft (R) C/C++ Optimizing Compiler Version 19.20.27323 for x64 
Copyright (C) Microsoft Corporation.  All rights reserved. 

