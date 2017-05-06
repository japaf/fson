Fortran 95 JSON Parser
======================
Written by Joseph Alevin. Original and description can be found [here](https://github.com/josephalevin/fson). This fork provides CMake installation (no tests, no examples). Original installation didn't work for some json documents (I am not sure why).

# Install

The library is installed in user-space (`${HOME}/lib/fson`) and (`${HOME}/include/fson`) by default.

```
mkdir build; cd build
cmake ..
cmake --build .
cmake --build . --target install
```

Fin.
