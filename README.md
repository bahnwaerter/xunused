# xunused
`xunused` is a tool to find unused C/C++ functions and methods across source files in the whole project.
It is built upon clang to parse the source code (in parallel). It then shows all functions that had
a definition but no use. Templates, virtual functions, constructors, functions with `static` linkage are
all taken into account. If you find an issue, please open a issue on https://github.com/mgehre/xunused or file a pull request.

xunused is compatible with LLVM and Clang versions 13 to 17.

## Building and Installation
First download or build the necessary versions of LLVM and Clang with development headers.
On Debian and Ubuntu, this can easily be done via [http://apt.llvm.org](http://apt.llvm.org) and `apt install llvm-17-dev libclang-17-dev`.
Then build via
```
mkdir build
cd build
cmake ..
make
```

## Run it
To run the tool, provide a [compilation database](https://clang.llvm.org/docs/JSONCompilationDatabase.html).
By default, it will analyze all files that are mentioned in it.
```
cd build
./xunused /path/to/your/project/compile_commands.json
```
You can specify the option `-filter` together with a regular expressions. Only files who's path is matching the regular
expression will be analyzed. You might want to exclude your test's source code to find functions that are only used by tests but not any other code.

If `xunused` complains about missing include files such as `stddef.h`, try adding `-extra-arg=-I/usr/include/clang/17/include` (or similar) to the arguments.

## Debug it
To further develop xunused or to debug errors, the built-in debug output and statistics can be used.
Therefore, LLVM must be compiled with enabled assertions as follows.
```
cmake -DLLVM_ENABLE_ASSERTIONS=ON ...
```

Once LLVM is built with enabled assertions, xunused can be built in Debug configuration as follows.
```
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Debug ..
make
```

The debug output and statistics of can now be enabled when calling xunused as follows.
```
./xunused -debug-only=xunused -stats /path/to/your/project/compile_commands.json
```
