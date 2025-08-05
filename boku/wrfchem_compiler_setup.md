# WRF-Chem Compiler Setups

Choosing the right compiler and setting the correct environment variables and flags is crucial for a successful WRF-Chem compilation. This guide provides an overview of common compiler setups.

## General Advice

-   **Consistency is Key**: Use the same compiler suite (e.g., Intel Fortran and Intel C/C++) for all components: WRF, NetCDF, and other libraries. Mixing compilers can lead to issues.
-   **Check `configure.wrf`**: After running `./configure`, always inspect the generated `configure.wrf` file. This file contains the compiler flags and paths that will be used for compilation. You may need to manually edit this file to set the correct paths to your libraries.
-   **Module Systems**: If you are on an HPC system with a module environment, make sure to load the same compiler, MPI, and library modules for both compiling and running WRF-Chem.

## GNU Compilers (gfortran/gcc)

The GNU Compiler Collection (GCC) is a popular free and open-source option.

-   **Configuration**: When you run `./configure`, select one of the `gfortran` options.
-   **Environment Variables**:
    ```bash
    export CC=gcc
    export CXX=g++
    export FC=gfortran
    ```
-   **Performance**: While free and widely available, GNU compilers may not always produce the most performant WRF-Chem executables compared to commercial compilers like Intel's.
-   **Common Flags**:
    -   `-O2` or `-O3`: Optimization levels.
    -   `-mtune=native` or `-march=native`: Optimize for the specific architecture of the machine you are compiling on.
    -   `-fconvert=big-endian`: May be needed for some data formats.

## Intel Compilers (ifort/icc)

The Intel compilers are a popular choice for HPC and are known for generating highly optimized code for Intel processors.

-   **Configuration**: Select one of the `ifort` options during `./configure`.
-   **Environment Variables**:
    ```bash
    export CC=icc
    export CXX=icpc
    export FC=ifort
    ```
-   **Performance**: Generally considered to produce the fastest executables on Intel-based systems.
-   **Common Flags**:
    -   `-O2` or `-O3`: Optimization levels.
    -   `-xHost`: Generates instructions for the highest instruction set available on the compilation host.
    -   `-ipo`: Interprocedural optimization.
    -   `-convert big_endian`: For data format compatibility.

## PGI / NVIDIA Compilers (pgfortran/pgcc)

The PGI compilers (now part of the NVIDIA HPC SDK) are another high-performance option, especially on systems with NVIDIA GPUs (for WRF, not necessarily WRF-Chem's CPU code).

-   **Configuration**: Select one of the `pgi` options during `./configure`.
-   **Environment Variables**:
    ```bash
    export CC=pgcc
    export CXX=pgc++
    export FC=pgfortran
    ```
-   **Common Flags**:
    -   `-O2` or `-O3`: Optimization levels.
    -   `-fast`: A macro for a set of performance-enhancing flags.
    -   `-Mbyteswapio`: For big-endian/little-endian conversion.

By understanding these compiler options, you can make an informed choice for your system and ensure a smoother compilation process for WRF-Chem.
