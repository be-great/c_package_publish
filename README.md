# Adding c project using APT package installer
 
 Example function that adds two number

## Step 1: Organize Your Project

Create a directory structure for your project:

    mylibrary/
    |-- DEBIAN/
    |   `-- control
    |-- usr/
    |   |-- include/
    |   |   `-- main.h
    |   `-- lib/
    |       `-- libmylibrary.so
    |-- src/
    |   `-- _add.c
    `-- Makefile

## Step 2: Write Control File

Inside the DEBIAN directory, create a control file:

    $ vim control
<be>

    Package: mylibrary
    Version: 1.0
    Architecture: amd64
    Maintainer: Your Name <your@email.com>
    Description: A simple library with an _add function.

## Step 3: Write Header and Source Files

    $ vim usr/include/ahedlib.h
<br>

    #ifndef MAIN_H
    #define MAIN_H

    int _add(int a, int b);

    #endif /* MAIN_H */
<br>


    $ vim src/_add.c
<br>


    #include "main.h"
    int _add(int a, int b)
    {
        return (a + b);
    }

## Step 4: Write Makefile

Create a Makefile to compile your code:

    $ vim Makefile
<br>


    CC=gcc
    CFLAGS=-Wall
    LDFLAGS=-shared

    all: libmylibrary.so

    libmylibrary.so: _add.o
        $(CC) $(LDFLAGS) -o $@ $^

    _add.o: src/_add.c
        $(CC) -fPIC -c $(CFLAGS) -I/usr/include $< -o $@

    clean:
        rm -f libmylibrary.so _add.o


    clean:
        rm -f libmylibrary.so _add.o

## Step 5: Build the Library

Run make to build your shared library:

    $ make
    $ mv libmylibrary.so usr/lib

## Step 6: Create Debian Package

    $ dpkg-deb --build mylibrary
## Step 7: install the package 

    $ apt install ./mylibrary.deb
## Step 8: use it on your program 

    $ vim main.c
<br>


    #include <ahedlib.h>
    #include <stdio.h>

    int main() {
        int result = _add(7, 8);
        printf("Result: %d\n", result);
        return 0;
    }

<br>


    $ gcc main.c -o main -l mylibrary

## Step 9: publish it on github repo

 1- upload the mylibrary.deb
    
    $ git add mylibrary.deb
    $ git commit -m "add the package" 
    $ git push
 2- release the package 

    A- Go to your GitHub repository.
    B- Click on the "Releases" tab.
    C- Click on "Draft a new release."
    D- Tag version should be the version you just created (v<version>).
    E- Title and description can be filled in with relevant information.
    F- Attach your Debian package (.deb file) to the release.
