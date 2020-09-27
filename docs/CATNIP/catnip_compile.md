# Compiling

If a binary file is not available for you specific architecture and distribution, you can download the source code and build the program from scratch. The code is simple enough to compile after downloading all you really need is cmake and the g++ compiler. It has not been tested on windows or OSX.  

Simply cd into the `QC_Tools` directory and run the following commands

    cd QC_TOOLS
    mkdir build
    cd build
    cmake -DCMAKE_BUILD_TYPE=Release ../
    make
    sudo make install

This should generate an executable called calc_J which is placed in `/usr/local/bin`. If you do not have sudo permissions see below. To understand how to run the code simply type:

    calc_J -h

Or take a look at the tutorials. 

## Installing without sudo permissions

If you do not have sudo permissions you can access the executable directly within the build directory without calling `sudo make install`. The above executable help command can then be run as:  

    ./QC_Tools/build/calc_J -h

If you want to install to a specific folder. Say for instance I want to install to my home directory `/home/josh`. I could do this as:

    cd QC_TOOLS
    mkdir build
    cd build
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX:PATH=/home/josh ../
    make
    make install

Here I do not need to use sudo as it is in my local home directory where I have access permissions. 

### Note

The build process has been updated to use cmake, you will need to have version 2.8 or higher to compile.

# CMake Flags

Additional flags can be passed to cmake for additional customization if needed for instance instead of calling cmake alone as shown above `cmake ../` one can insert flags like so:

    cmake -DLOG_LEVEL=1 ../

## Flags

Each flag must be prepended with a -D when calling cmake. 

*LOGLEVEL* - By setting variable to an integer value of 1 or more it will display additional information when calc_J is called. The level of verbosity increases with the integer value passed to it. 

    -DLOG_LEVEL=2

*CMAKE_INSTALL_PREFIX:PATH* - Sets the install directory, to a user defined one. 

    -DCMAKE_INSTALL_PREFIX::PATH=/usr/local/

*ENABLE_TESTS* - Allows for the compilation of several unit tests, by default is turned off. 

    -DENABLE_TESTS=ON

*ENABLE_INTEGRATION_TESTS=ON* - Will allow the compilation of several integration tests.

    -DENABLE_INTEGRATION_TESTS=ON

*DOWNLOAD_TUTORIAL_FILES* - Will download the tutorial files from google drive. 
