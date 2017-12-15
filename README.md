# How To Build CDO


## Unpack the prerequisite tarball and enter directory
```bash
tar -xzvf libs4cdo-0.0.11.tar.gz
cd libs4cdo-0.0.11
```

## Set environment variables for compilers
```bash
export CC=gcc
export CXX=g++
export FC=gfortran
export F77=fort77
```

## Set environment variable for the desired installation directory
```bash
export CDODIR=/usr/local/cdo_dir
```

## Install zlib
```bash
cd zlib-1.2.3/
./configure --prefix=$CDODIR --shared
sudo make all install
```

## Update built-in environment variable for finding non-standard libraries
```bash
export LD_LIBRARY_PATH=$CDODIR/lib:$LD_LIBRARY_PATH
```

## Install szip
```bash
cd ../szip-2.1/
./configure --prefix=$CDODIR
sudo make all install
```

## Install Jasper
```bash
cd ../jasper-1.900.1/
./configure --prefix=$CDODIR --enable-shared --with-pic
sudo make all install
```

## Install Proj
```bash
cd ../proj-4.8.0/
./configure --prefix=$CDODIR --without-mutex --without-jni
sudo make all install
```

## Install HDF5
```bash
cd ../hdf5-1.8.15/
./configure --prefix=$CDODIR --with-zlib=$CDODIR
 sudo make all install
```

## Install NetCDF-3
```bash
cd ../netcdf-3.6.3/
./configure --prefix=$CDODIR --enable-c-only --enable-shared
sudo make all install
```

## Install NetCDF-4
```bash
cd ../netcdf-4.3.3.1/
CPPFLAGS=-I$CDODIR/include LDFLAGS=-L$CDODIR/lib ./configure --prefix=$CDODIR --enable-shared --enable-netcdf-4 --with-pic --disable-doxygen
sudo make all install
```

## Install GRIB API
```bash
cd ../grib_api-1.13.1/
autoreconf -ivf
CFLAGS='-DPIC -fPIC' ./configure --prefix=$CDODIR --with-jasper=$CDODIR
sudo make all install
```

## Unpack CDO
```bash
tar -xzvf cdo-1.8.2.tar.gz 
```

## Install CDO
```bash
cd ../cdo-1.8.2/
./configure --prefix=$CDODIR --with-netcdf=$CDODIR --with-hdf5=$CDODIR --with-grib_api=$CDODIR --with-proj=$CDODIR
sudo make all install
```

## Update PATH environment variable
```bash
export PATH=/usr/local/cdo_dir/bin:$PATH
```

## Check to ensure `cdo` command works
```bash
cdo -V
```

## Update bashrc file 
```bash
echo 'PATH=/usr/local/cdo_dir/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```
