# How To Build CDO

## System Requirements

### Operating System

This guide is tailored for Debian-based Linux distros. 

### Compilers

Fortran 77 and 90/95 compilers are required for the installation of the GRIB API and hence, CDO. By default, these may not be installed so if you get no response from `which gfortran` and/or `which fort77`, install them by typing: `sudo apt-get install gfortran` and/or `sudo apt-get install fort77`; appropriately. You will also need the C compilers: gcc and g++.

## Part 0 - Downloading CDO and it's Prerequisites

This guide is tailored for CDO 1.8.2 and its prerequisite libs. At this time, these can be obtained via:

```bash
wget https://code.mpimet.mpg.de/attachments/download/10301/libs4cdo-0.0.11.tar.gz
wget https://code.mpimet.mpg.de/attachments/download/14686/cdo-1.8.2.tar.gz
```

**Note:** If you decide to use different versions of these packages then you must change the commands I've provided accordingly.


## Part 1 - Installing Prerequisite Libraries

**Note:** make sure you are running these commands from a *user* owned directory like, `~/Downloads/cdo_config_dir/`, the one that I used. Further note, that CDO and all other libraries will be installed in a *root* owned directory so `make all install` must be run by the *superuser*, i.e. `sudo make all install`.

### Unpack the prerequisite tarball and enter directory
```bash
tar -xzvf libs4cdo-0.0.11.tar.gz
cd libs4cdo-0.0.11
```

### Set environment variables for compilers
```bash
export CC=gcc
export CXX=g++
export FC=gfortran
export F77=fort77
```

### Set environment variable for the desired installation directory
```bash
export CDODIR=/usr/local/cdo_dir
```

### Install zlib
```bash
cd zlib-1.2.3/
./configure --prefix=$CDODIR --shared
sudo make all install
```

### Update built-in environment variable for finding non-standard libraries
```bash
export LD_LIBRARY_PATH=$CDODIR/lib:$LD_LIBRARY_PATH
```

### Install szip
```bash
cd ../szip-2.1/
./configure --prefix=$CDODIR
sudo make all install
```

### Install Jasper
```bash
cd ../jasper-1.900.1/
./configure --prefix=$CDODIR --enable-shared --with-pic
sudo make all install
```

### Install Proj
```bash
cd ../proj-4.8.0/
./configure --prefix=$CDODIR --without-mutex --without-jni
sudo make all install
```

### Install HDF5
```bash
cd ../hdf5-1.8.15/
./configure --prefix=$CDODIR --with-zlib=$CDODIR
 sudo make all install
```

### Install NetCDF-3
```bash
cd ../netcdf-3.6.3/
./configure --prefix=$CDODIR --enable-c-only --enable-shared
sudo make all install
```

### Install NetCDF-4
```bash
cd ../netcdf-4.3.3.1/
CPPFLAGS=-I$CDODIR/include LDFLAGS=-L$CDODIR/lib ./configure --prefix=$CDODIR --enable-shared --enable-netcdf-4 --with-pic --disable-doxygen
sudo make all install
```

### Install GRIB API
```bash
cd ../grib_api-1.13.1/
autoreconf -ivf
CFLAGS='-DPIC -fPIC' ./configure --prefix=$CDODIR --with-jasper=$CDODIR
sudo make all install
```

## Part 2 - Installing CDO

### Return to directory with tars and unpack CDO
```bash
cd ../..
tar -xzvf cdo-1.8.2.tar.gz 
```

### Install CDO
```bash
cd cdo-1.8.2/
./configure --prefix=$CDODIR --with-netcdf=$CDODIR --with-hdf5=$CDODIR --with-grib_api=$CDODIR --with-proj=$CDODIR
sudo make all install
```

### Update PATH environment variable
```bash
export PATH=/usr/local/cdo_dir/bin:$PATH
```

### Check to ensure `cdo` command works
```bash
cdo -V
```

### Update bashrc file 
```bash
echo 'PATH=/usr/local/cdo_dir/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```
