## How to cross compile your (SDL-) apllication for Windows, from a Gnu/Linux based host with SCons ##
1.	Get **MinGW** for your distribution.
2.	Get the **SDL-devel*mingw32.tar.gz** package from the libsdl.org download page, and extract it.
3.	Then, in the **Makefile**, change `CROSS_PATH` so that it points to your MinGW folder.  
         On my Archlinux based PC this would be:   
         `CROSS_PATH := /usr/i486-mingw32`
4.	Run **sudo make cross**, this will install the SDL .dll/.a and include files into the mingw folder.  
        (You could also skip step 3 and 4 and just copy the files manually, if you want to)
5.  Look at the SConscript file in the 'source' directory of this package.  
  
    ```python
    #cross compile SConscript file  
    # run 'scons platform=windows' to get a windows executable  
    # run 'scons' or 'scons platform=linux' to get a linux executable  
    
    if ARGUMENTS.get('platform', 'linux') == 'windows': #windows build
    env = Environment(tools = [], LIBS=['mingw32', 'SDLmain', 'SDL'])
        env.Tool('crossmingw', toolpath = ['../scons-tools']) # load helper tool to setup the MinGW environment
    else: #linux build
        env = Environment(LIBS=['SDL'])
    
    env.Program(target = 'main', source = ['main.cpp'])
    ```  

6.  Now it get's a bit hacky:
    For your Windows distribution-package, you basically have to copy every *.dll file from the MinGW folder (you previously set) to where the .exe is (the 'build' folder) and include all that into 
your Windows package e.g. a .zip file.
