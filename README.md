cmake-ide
==========

[![Build Status](https://travis-ci.org/atilaneves/cmake-ide.svg?branch=master)](https://travis-ci.org/atilaneves/cmake-ide)

`cmake-ide` enables autocompletion and on-the-fly syntax checking
in Emacs for CMake projects with minimal configuration. It depends
on [flycheck](https://github.com/flycheck/flycheck) and
[auto-complete-clang](https://github.com/brianjcj/auto-complete-clang).
Support might be added for other packages later.

It works by running CMake in Emacs in order to obtain the necessary
`-I` and `-D` compiler flags to pass to the other tools. Since all
the dependencies are specified in the CMake scripts, there is no
need to maintain a parallel dependency tracking system for Emacs.
Just ask CMake.

Features
--------
* Sets variables for `auto-complete-clang` and `flycheck` for a CMake
  project automagically.
* Automatically reruns CMake when a file is saved. Great when using
CMake globs, but needs `cmake-ide-dir` to be set.
* `cmake-ide-delete-file` allows you to have the same convenience when
deleting files. I can't figure out a better way to do this. Obviously
simply deleting the file means having to run CMake again manually for
it to register the change in the list of files to be compiled
* If `cmake-ide-dir` is set, `cmake-ide-compile` compiles in the build directory.
Automatically detects Ninja and Make builds and sets the compile command accordingly.


Usage
-----

Add this to your `.emacs` / `init.el`:

    (cmake-ide-setup)

If `cmake-ide-clang-flags-c` or `cmake-ide-flags-c++` are set, they
will be added to `ac-clang-flags`.  These variables should be
set. Particularly, they should contain the system include paths
(e.g. `'("-I/usr/include/c++/4.9.1" "...")`. For a system with gcc,
you can get this information by running `gcc -v -xc++ /dev/null
-fsyntax-only` (it's the same prerequisite for `auto-complete-clang`
to work, since that's how clang itself works).

And... that's it. It works by calling cmake and parsing the resulting
JSON file with compiler flags.  Set `cmake-ide-dir` to your project's
root directory and you won't have to call CMake manually again (except
for the first time to specify options). Best done with
[directory local variables](https://www.gnu.org/software/emacs/manual/html_node/emacs/Directory-Variables.html).


Related Projects:
----------------
* [cpputils-cmake](https://github.com/redguardtoo/cpputils-cmake).
* [My CMake modules for emacs. This project is better](https://github.com/atilaneves/cmake_modules).
