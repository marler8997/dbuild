# Bud (Build D)

A tool to build D programs and libraries.

# TODO

I think I'll combine the `rund` repo and this repo since alot of their code/logic will be shared.

# Notes

Bud standardizes build output locations.  Bud can build a package once and that output can be re-used by any other program being built with bud.  I need to document these standards and the benefits of them.

# Example
```
bud <somepath>/helloworld.d
```

Creates:
```
# the compiled executable
<somepath>/bin/<compile-environment-hash>/helloworld

# convenient link to the exe so you don't have to know the has to run the exe
<somepath>/bin/helloworld -> <compile-environment-hash>/helloworld

# the json file generated by the compiler containing all the information about what was compiled
<somepath>/bin/<compile-environment-hash>/lastBuild.json

# all the information used to generate the <compile-environment-hash>
<somepath>/bin/<compile-environment-hash>/env.json

# the location to store object files
<somepath>/bin/<compile-environment-hash/obj
```

# Source Compiler Directives

See `rund`.  Bud will also support these.

# Bud Standards

Bud defines 2 kinds of modules

1. Entry Point Modules
2. Package Modules

An "entry point module" is any module with a `main` function. These modules should be the main source file passed to bud, but should never be imported by another module.  For this reason, entry point modules should not have a module name, this also allows bud to distinguish these two types of modules quickly based on whether it contains a module name. There is one exception, if the filename of the module conflicts with a package, you can use `module __none;` to reolsve the conflict.

Package modules are the opposite, they always have a module name. Bud is more strict when it comes to module names.  The module name must match the name of the file, i.e. a module named `foo.bar.baz` should be in `foo/bar/baz.d`.  Another requirement for bud is that the root package directory should live inside its own subdirectory with no other files.  In the previous example, the root directory `foo` should live in a directory with no other files or directories.  The reason for this is that the compiler interface to include a "library path" is to pass in the directory where the package source lives, and bud needs to be able to pass in each library independently.  It can only do this if each package is isolated in its own directory.  Also, it's recommended that all files within a package folder only contain D source files for that package.
