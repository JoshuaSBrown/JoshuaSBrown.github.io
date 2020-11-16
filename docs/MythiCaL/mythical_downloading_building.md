---
layout: mythical_default
--- 

# Downloading & Building 

_MythiCaL_ is hosted [here](https://github.com/JoshuaSBrown/MythiCaL) on github. It can be downloaded,
built and installed with just a few CMake commands. 

The library makes use of c++14 features so requires gcc 6 or a compiler with
similar support. The only other dependency used by _MythiCaL_ is Catch2 which
will automatically be built with _MythiCaL_ if it is not already installed on
the system. 

    git clone --recursive https://github.com/JoshuaSBrown/Mythical
    cd Mythical
    cmake -S . -B build
    cmake -DCMAKE_BUILD_TYPE=Release --build build
    cmake --install build
    
