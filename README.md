# Valgrind Exercise

This repository contains a simple C++ program that simulates an analog sensor reading. The purpose of this exercise is to identify and fix memory-related issues using Valgrind.

## Standard install via command-line
```bash
# Configure the project and generate a native build system:
  # Must re-run this command whenever any CMakeLists.txt file has been changed.
  cmake -S ./ -B build/
# Compile and build the project:
  # rebuild only files that are modified since the last build
  cmake --build build/
  # or rebuild everything from scracth
  cmake --build build/ --clean-first
  # to see verbose output, do:
  cmake --build build/ --verbose
# Run program:
  ./build/app/shell-app
# Clean
  cmake --build build/ --target clean
# Clean and start over:
  rm -rf build/
```

## Valgrind Output Analysis

The initial Valgrind output identified an uninitialized value in the `main` function and a memory leak in the `AnalogSensor` class. These issues were fixed by initializing the `terminator` variable in `main.cpp` and freeing the dynamically allocated `readings` vector in `AnalogSensor.cpp`.

## What happens when the executable is linked statically? Does Valgrind still detect those same bugs? Why or why not?

When an executable is linked statically, all the libraries it depends on are included within the executable itself. This means that the executable does not rely on shared libraries on the system to run. Instead, it contains all the code it needs to execute.

Valgrind operates by monitoring the calls an application makes to the system's memory allocation functions. It checks for memory leaks, invalid memory access, and other memory-related issues.

The bugs that Valgrind detected in the program were related to:

    An uninitialized value.
    Memory leaks due to dynamically allocated memory not being freed.

These bugs are inherent to the code written and are not influenced by how the executable is linked (statically or dynamically). Therefore, even if the executable is linked statically, Valgrind would still detect these same bugs, because they are present in the program's logic and memory management, not in its dependencies or how it's linked.

However, it's worth noting that static linking can sometimes introduce complications when using tools like Valgrind. Some static libraries might have known memory management behaviors that appear as issues in Valgrind but are actually intentional and not problematic. In such cases, Valgrind might report false positives. But in the context of the bugs found, static linking would not hide or change the bugs Valgrind detected.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.