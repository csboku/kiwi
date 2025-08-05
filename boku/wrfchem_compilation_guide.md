# WRF-Chem Compilation: A Comprehensive Guide

This guide provides a detailed, step-by-step walkthrough for compiling WRF-Chem and its dependencies from source. We will use specific, tested versions of the required libraries and compilers to ensure a successful build.

## 1. Software Versions

This guide is based on the following software versions, which have been reported to work well together for recent versions of WRF-Chem (e.g., v4.6.0):

-   **Compilers**:
    -   Intel oneAPI (ifx/icx): 2024.1.0
    -   GNU (gfortran/gcc): 10.3.0 or newer
-   **Libraries**:
    -   HDF5: 1.14.4
    -   pnetcdf: 1.13.0
    -   netcdf-c: 4.9.2
    -   netcdf-fortran: 4.6.1
-   **WRF-Chem**: v4.6.0

## 2. Directory Structure and Environment

First, let's set up our directory structure and environment variables.

```bash
# Create a directory for the WRF-Chem build
mkdir ~/wrf-chem-build
cd ~/wrf-chem-build

# Create directories for the libraries and the WRF source code
mkdir libs
mkdir wrf-sources

# Set the root directory for the build
export WRF_BUILD_ROOT=~/wrf-chem-build

# Set environment variables for the compilers.
# Choose ONE of the following blocks.

# For Intel oneAPI (ifx/icx)
export CC=icx
export CXX=icpx
export FC=ifx

# For GNU (gfortran/gcc)
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

## 3. Compiling the Libraries

Now, we will compile each of the required libraries from source.

### HDF5

```bash
cd $WRF_BUILD_ROOT/wrf-sources
wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.14/hdf5-1.14.4/src/hdf5-1.14.4.tar.gz
tar -xzvf hdf5-1.14.4.tar.gz
cd hdf5-1.14.4
./configure --prefix=$HDF5_PATH --enable-fortran --enable-parallel
make
make install
```

### pnetcdf

```bash
cd $WRF_BUILD_ROOT/wrf-sources
wget https://parallel-netcdf.github.io/release/pnetcdf-1.13.0.tar.gz
tar -xzvf pnetcdf-1.13.0.tar.gz
cd pnetcdf-1.13.0
./configure --prefix=$PNETCDF_PATH
make
make install
```

### netcdf-c

```bash
cd $WRF_BUILD_ROOT/wrf-sources
wget https://github.com/Unidata/netcdf-c/archive/refs/tags/v4.9.2.tar.gz
tar -xzvf v4.9.2.tar.gz
cd netcdf-c-4.9.2
CPPFLAGS="-I$HDF5_PATH/include" LDFLAGS="-L$HDF5_PATH/lib" ./configure --prefix=$NETCDF_PATH --enable-netcdf-4 --enable-parallel-tests --disable-dap
make
make install
```

### netcdf-fortran

```bash
cd $WRF_BUILD_ROOT/wrf-sources
wget https://github.com/Unidata/netcdf-fortran/archive/refs/tags/v4.6.1.tar.gz
tar -xzvf v4.6.1.tar.gz
cd netcdf-fortran-4.6.1
CPPFLAGS="-I$NETCDF_PATH/include" LDFLAGS="-L$NETCDF_PATH/lib" ./configure --prefix=$NETCDF_PATH
make
make install
```

## 4. Compiling WRF-Chem

Now that all the dependencies are compiled, we can compile WRF-Chem itself.

```bash
cd $WRF_BUILD_ROOT/wrf-sources
# Clone the WRF-Chem repository
git clone https://github.com/wrf-model/WRF.git
cd WRF
git checkout v4.6.0

# Set environment variables for WRF-Chem
export WRF_CHEM=1
export WRF_KPP=1
export YACC='/usr/bin/yacc -d'
export FLEX_LIB_DIR=/usr/lib/x86_64-linux-gnu/ # Adjust for your system

# Configure WRF-Chem
./configure
# Select the appropriate option for your compiler and MPI setup.
# For example, for ifx/icx with distributed memory, you would choose the
# corresponding option.

# After configure finishes, it will create a configure.wrf file.
# Open this file and verify that the paths to the libraries are correct.

# Compile WRF-Chem
./compile em_real >& compile.log
```

After the compilation finishes, check the `compile.log` for any errors. If the compilation was successful, you will have the `wrf.exe`, `real.exe`, and other executables in the `main/` directory.

This guide provides a robust and tested workflow for compiling WRF-Chem. By using these specific software versions and following these steps carefully, you can minimize the chances of encountering compilation errors.
