---
layout: mythical_default
--- 

# Importing with CMake

MythiCaL is designed to be used as a library. Assuming it is installed correctly it should be fairly straightforward to build with it. Below we demonstrate a simple program built with mythical. 

Assuming we have a very simple cpp source file **example.cpp** which simply prints the version of the mythical library that it is being built with. The contents of which are shown below

```c++
#include <mythical/version.hpp>
#include <iostream>

int main() {
  std::cout << "This file has been built with:" << std::endl;
  std::cout << mythical::PROJECT_NAME << " " << mythical::PROJECT_VER << std::endl;
  return 0;
}
```

In the same directory we also have a **CMakeLists.txt** file. A total of five lines are needed. First the version of cmake supported is declared then the project name is declared. Next the find_pakage command is called, this will attempt to find the MythiCaL library. Finally, the source file is made into an executable and linked with the MythiCaL library. 

```CMake
cmake_minimum_required(VERSION 3.12)
project(example_project)
find_package(MythiCaL REQUIRED)
add_executable(example example.cpp)
target_link_libraries(example PUBLIC mythical::mythical)
```

Running the following commands in the terminal in the same directory as the **example.cpp** and the **CMakeLists.txt** file will yield the following. 

```bash
$ cmake -S . -B build
$ cmake --build build
$ ./build/example
This file has been built with:
mythical 0.0.0
```
