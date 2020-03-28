# **Building libharu with Visual Studio 2019**

## Precondition
 - libharu ver2.3.0: [http://libharu.org/](http://libharu.org/)
 - zlib ver1.2.11: [https://zlib.net/](https://zlib.net/)
 - libping ver1.6.37: [http://www.libpng.org/pub/png/libpng.html](http://www.libpng.org/pub/png/libpng.html)

Unzip these files in directory "build".
The directory structure is here.

    build                               # root for bulding
    ├─ libharu-RELEASE_2_3_0            # libharu
    ├─ lpng1637                         # libpng
    └─ zlib-1.2.11                      # zlib

It required that libpng and zlib has built with VS2019.
Reference: [Building libpng with Visual Studio 2019](https://gist.github.com/mryssng/98873a1588bfdae738dfb28a704c1522)

## Build libharu

 1. **Copy libpng and zlib**<br>
   libpng:
Copy "libpng16.lib" and "zlib.lib" from "build\lpng1637\projects\vstudio\x64\Release Library" to "build\libharu-RELEASE_2_3_0"

 2. **Edit Script**<br>
  Open file "build\libharu-RELEASE_2_3_0\script\Makefile.msvc_dll".<br>
  Edit as below.
     * Line #14
     `PNG_PREFIX   = ../../libpng`
     to
     `PNG_PREFIX   = ../lpng1637`
     
     * Line #18
     `ZLIB_PREFIX   = ../../zlib`
     to
     `ZLIB_PREFIX   = ../zlib-1.2.11`
        
     * Line #30
     `CFLAGS=/MD -nologo -O2 -Iinclude -Iwin32\include   -I"$(PNG_PREFIX)"\include -I"$(ZLIB_PREFIX)"\include -DHPDF_DLL_MAKE`
     to
     `CFLAGS=/MD -nologo -O2 -Iinclude -Iwin32\include -I"$(PNG_PREFIX)" -I"$(ZLIB_PREFIX)" -DHPDF_DLL_MAKE`
        
     * Line #33
     `LDFLAGS= /LIBPATH:$(PNG_PREFIX)\lib /LIBPATH:$(ZLIB_PREFIX)\lib /LIBPATH:win32\msvc libpng13.lib zlib.lib`
     to
     `LDFLAGS= /LIBPATH:$(PNG_PREFIX)\lib /LIBPATH:$(ZLIB_PREFIX)\lib /LIBPATH:win32\msvc libpng16.lib zlib.lib`

3. **Build with MS Build in  Visual Studio 2019 Tools**<br>
  Launch **"x64 Native Tools Command Prompt for VS 2019"** and change directory to "build\libharu-RELEASE_2_3_0"<br>
  Type below and `Enter`<br>
  
    `nmake -f script\Makefile.msvc_dll`<br>
    
    When the build is completed, you can see the below message.<br>
	```sh
	Finished generating code
	        rename libhpdf.lib libhpdf.lib
	        copy libhpdf.dll demo
	        1 file(s) copied.
   ```
    The directory structure after building is here.

	    build/ 
	    ├─ libharu-RELEASE_2_3_0/ 
	    │   ├─ cmake/ 
	    │   ├─ demo/ 
	    │   ├─ doc/ 
	    │   ├─ if/
	    │   ├─ include/
	    │   ├─ script/ 
	    │   ├─ src/ 
	    │   ├─ win32/
	    │   ├─ .gitignore 
	    │   ├─ CHANGES 
	    │   ├─ CMakeLists.txt 
	    │   ├─ INSTALL 
	    │   ├─ Makefile.am 
	    │   ├─ README 
	    │   ├─ README_cmake# 
	    │   ├─ build.mk 
	    │   ├─ buildconf.sh 
	    │   ├─ configure.in 
	    │   ├─ libharu.DevPackage.cmake 
	    │   ├─ libhpdf.dll 
	    │   ├─ libhpdf.exp 
	    │   ├─ libhpdf.lib 
	    │   ├─ libpng16.lib 
	    │   └─ zlib.lib
	    │   
	    ├─ lpng1637/
	    └─ zlib-1.2.11/ 

	Completed! 😄