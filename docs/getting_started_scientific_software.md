---
layout: sub_home
---
# Introduction

## Why use C++ for scientific software development?

This library is written in C++. You might ask, "why use C++ when there are other easier languages to use?", for instance, Python or Matlab. This is a good question to ask before continuing. Python and Matlab among others are nice because they are higher level languages meaning much of the complexity under the software 'hood' is hidden, furthermore it is easy to plot nice figures quickly. If you are a scientist and you want something quick, then Python and Matlab will probably be a better choice, however if you want to develop a software package that is **reusable** and efficient C++ might be a better choice. In C++ the developer is given detailed control of how the software interacts with the hardware. The added control does however come at the expense of complexity.

## How to write a program

Before trying to understand C++ one should have at least a basic grasp of how software languages work. A software languages is just human readable logic. This means that a human can read and write the logic in an easy to understand manner. Programs are written in this human readable language and are simply made up of a .txt files. There is nothing inherently special about the .txt file. How does one actually execute a program then? Well the computer cannot read the .txt files them selves and execute them the computer has its own language called binary which is made up of 1s and 0s. To execute a program the .txt files must first be translated to binary and then executed. This so called translator is called a compiler. In Python and Matlab the .txt files are compiled on the fly and are not broken up into an extra step. In C++ an open source compiler is g++, there are others some are proprietary. The compiler takes the program .txt files and translates it into machine code or "binary" and outputs it as a binary file. If you open the binary file it will appear as a mish-mash of letters which are not readable. This binary file can however be executed.

## An example "Hello World"

To compile a program on linux, you will first need to have g++ installed. Create a .txt file but instead of using the .txt extension use a .cpp extension to indicate you will be writing in the C++ language. The extension does not do anything special it just lets you, the developer, know that the file is written in the C++ language. Below are the contents of our helloworld.cpp file. 

    #include <iostream>                                                                                                                                                                                   
    int main() {
        std::cout << "Hello World" << std::endl;
        return 0;
    }   

The "#include <iostream>" is importing commands from the standard library. The standard library contains certain functions, and classes that are commonly used. In this case the include statement is including the input output stream "iostream" header file which contains the "cout" and "endl" objects, which allows one to print to the terminal. Every program must have a main function which defines the body of the program. The return statement with the value 0 indicates the program executed correctly. To compile this program one would enter the following in the terminal. 

    g++ hello.cpp -o hello

This would take the human readable "hello.cpp" file and output it (hence the -o) to the hello file. The "hello" file can then be executed by typing

    ./hello

this should print to the terminal "Hello World". There you have compiled a program in c++. 
