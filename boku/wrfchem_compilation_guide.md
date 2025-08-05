# WRF-Chem Compilation: A Comprehensive Technical Guide

This guide provides a detailed, step-by-step walkthrough for compiling WRF-Chem and its dependencies from source. It is intended to be a comprehensive resource for ensuring a successful build.

## 1. Recommended Software Versions

This guide is based on the following software versions, which have been reported to work well together for recent versions of WRF-Chem (e.g., v4.6.0):

-   **WRF-Chem**: v4.6.0
-   **Compilers**:
    -   Intel oneAPI (ifx/icx): 2024.1.0
    -   GNU (gfortran/gcc): 10.3.0 or newer
-   **MPI Libraries**:
    -   OpenMPI: 4.1.x
    -   MPICH: 3.4.x
-   **I/O Libraries**:
    -   HDF5: 1.14.4
    -   pnetcdf: 1.13.0
    -   netcdf-c: 4.9.2
    -   netcdf-fortran: 4.6.1

## 2. Environment Setup: The Foundation

A successful WRF-Chem build starts with a properly configured environment.

### On HPC Systems: Use Environment Modules

If you are on an HPC system, **always** use the provided environment modules. Do not compile your own dependencies unless absolutely necessary.

```bash
# Example of setting up the environment on an HPC system
module purge
module load intel/2024.1.0
module load openmpi/4.1.5
module load hdf5/1.14.4
module load netcdf/4.9.2

# The module system will typically set the necessary environment variables
# (e.g., CC, FC, NETCDF_PATH, etc.) automatically.
```

### Local Machine Setup

If you are compiling on a local machine, you will need to set the environment variables manually.

```bash
# Create a directory for the WRF-Chem build
mkdir ~/wrf-chem-build
cd ~/wrf-chem-build

# Create directories for the libraries and the WRF source code
mkdir libs
mkdir wrf-sources

# Set the root directory for the build
export WRF_BUILD_ROOT=~/wrf-chem-build

# Set environment variables for the compilers (choose one block)
# --- Intel oneAPI (ifx/icx) ---
export CC=icx
export CXX=icpx
export FC=ifx
# --- GNU (gfortran/gcc) ---
# export CC=gcc
# export CXX=g++
# export FC=gfortran

# Set environment variables for the library installation paths
export HDF5_PATH=$WRF_BUILD_ROOT/libs/hdf5
export PNETCDF_PATH=$WRF_BUILD_ROOT/libs/pnetcdf
export NETCDF_PATH=$WRF_BUILD_ROOT/libs/netcdf

# Add the library paths to LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$HDF5_PATH/lib:$PNETCDF_PATH/lib:$NETCDF_PATH/lib:$LD_LIBRARY_PATH
```

## 3. Compiling Dependencies (Local Machine)

If you are on a local machine, compile the libraries in the following order.

### HDF5

```bash
cd $WRF_BUILD_ROOT/wrf-sources
wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.14/hdf5-1.14.4/src/hdf5-1.14.4.tar.gz
tar -xzvf hdf5-1.14.4.tar.gz
cd hdf5-1.14.4
./configure --prefix=$HDF5_PATH --enable-fortran --enable-parallel
make && make install
```

### pnetcdf

```bash
cd $WRF_BUILD_ROOT/wrf-sources
wget https://parallel-netcdf.github.io/release/pnetcdf-1.13.0.tar.gz
tar -xzvf pnetcdf-1.13.0.tar.gz
cd pnetcdf-1.13.0
./configure --prefix=$PNETCDF_PATH
make && make install
```

### netcdf-c

```bash
cd $WRF_BUILD_ROOT/wrf-sources
wget https://github.com/Unidata/netcdf-c/archive/refs/tags/v4.9.2.tar.gz
tar -xzvf v4.9.2.tar.gz
cd netcdf-c-4.9.2
CPPFLAGS="-I$HDF5_PATH/include" LDFLAGS="-L$HDF5_PATH/lib" ./configure --prefix=$NETCDF_PATH --enable-netcdf-4 --enable-parallel-tests --disable-dap
make && make install
```

### netcdf-fortran

```bash
cd $WRF_BUILD_ROOT/wrf-sources
wget https://github.com/Unidata/netcdf-fortran/archive/refs/tags/v4.6.1.tar.gz
tar -xzvf v4.6.1.tar.gz
cd netcdf-fortran-4.6.1
CPPFLAGS="-I$NETCDF_PATH/include" LDFLAGS="-L$NETCDF_PATH/lib" ./configure --prefix=$NETCDF_PATH
make && make install
```

## 4. Compiling WRF-Chem

1.  **Get the source code**:
    ```bash
    cd $WRF_BUILD_ROOT/wrf-sources
    git clone https://github.com/wrf-model/WRF.git
    cd WRF
    git checkout v4.6.0
    ```
2.  **Set WRF-Chem environment variables**:
    ```bash
    export WRF_CHEM=1
    export WRF_KPP=1
    export YACC='/usr/bin/yacc -d'
    export FLEX_LIB_DIR=/usr/lib/x86_64-linux-gnu/ # Adjust for your system
    ```
3.  **Configure and Compile**:
    ```bash
    ./configure # Select the appropriate option for your compiler and MPI
    ./compile em_real >& compile.log
    ```

## 5. Common Pitfalls and Best Practices

-   **MPI Library Choice**: For performance, OpenMPI and its derivatives (MVAPICH, MPT) are generally recommended over MPICH for WRF-Chem.
-   **Compiler Consistency**: You **must** use the same compiler suite (and version, if possible) for all dependencies and for WRF-Chem itself.
-   **Start Clean**: Before any compilation attempt, always run `./clean -a` to remove old files.
-   **Check `configure.wrf`**: After running `./configure`, manually inspect the `configure.wrf` file to ensure all paths and flags are correct.
-   **`compile.log` is Your Friend**: If the compilation fails, the `compile.log` file is the first place to look for errors.

This guide provides a robust and tested workflow for compiling WRF-Chem. By paying close attention to software versions and environment setup, you can significantly increase your chances of a successful build.