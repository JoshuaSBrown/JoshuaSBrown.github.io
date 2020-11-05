---
layout: mythical_default
--- 

# Downloading & Building 

## Recommended for the General User

## (For Developers or Unique Architectures) Building from Source

    git clone --recursive https://github.com/JoshuaSBrown/Mythical
    cd Mythical
    cmake -S . -B build
    cmake -DCMAKE_BUILD_TYPE=Release --build build
    cmake --install build
    
