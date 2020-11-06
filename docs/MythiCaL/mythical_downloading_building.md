---
layout: mythical_default
--- 

# Downloading & Building 

MythiCaL is hosted [here](https://github.com/JoshuaSBrown/MythiCaL) on github. It can be downloaded,
built and installed with just a few CMake commands.

    git clone --recursive https://github.com/JoshuaSBrown/Mythical
    cd Mythical
    cmake -S . -B build
    cmake -DCMAKE_BUILD_TYPE=Release --build build
    cmake --install build
    
