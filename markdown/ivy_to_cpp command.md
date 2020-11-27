[[command|Command]]: `ivy_to_cpp`

This command performs [[code extraction]] to C++.

The extracted code can often be customized with [[attribute|attributes]].

Given a mandatory `isolate=<name>` argument, `cpp_to_ivy` creates a C++ class for that [[isolate]], which can then be used one of three ways:

  - With `target=class`, a standalone class is emitted, which can be embedded in a C++ program
  - With `target=repl`, the class is connected to a read-eval-print loop (REPL) which allows manual interaction and testing.
  - With `target=test`, the class is connected to a randomized tester

The command takes several other options:

`classname=cname`

This gives the name of the extracted C++ class. The default is the name of the main IVy file, without the `.ivy` extension. The names of the extracted header and implementation files are `cname.h` and `cname.cpp` respectively.

`build=boolean`

If `true`, this option causes the extracted C++ to be compiled. In case of `repl` or `test` targets, the code is also linked into an executable file. On Unix the name of the executable is the same as the class name, whereas on Windows it is `cname.exe`, where `cname` is the class name.

`compiler={g++,cl}`

This option determines the compiler used to build the code. The default is `g++` on Unix and `cl` on Windows.

`trace=boolean`

This option causes statements producing trace information on `stdout` to be inserted in the extracted code.

`main=cname`

Determines the name of the main function, if one is generated. The default is `main`.

`stdafx=boolean`

Causes the file `stdafx.h` to be included in the first line of the implementation file.

`outdir=directory`

Causes output files to be generated in directory. Default is the current directory.