---
title: Install Wgrib2 on Ubuntu/ Red Hat or any other Linux OS
author: wxguy
date: 2023-01-10 21:10:00 +0530
categories: [Info, collections]
tags: [wgrib2,linux,weather,grib2]
pin: false
toc: true
comments: true
render_with_liquid: true
media_subpath: /assets/img/wgrib2/
image:
  path: ncl_grib2_plot.png
  alt: Sample Plot of GRIB2 Data using NCL. (Credit:- https://www.ncl.ucar.edu).
---

-------

## About GRIB File

If you are a Meteorologist or Climate Scientist or Engineer working on a forecast or Reanalysis data set, you would have probably come across the `grib` file format. It is a shortened name for "**G**eneral **R**egularly distributed **I**nformation in **B**inary form" which is a WMO standard for storing and transferring gridded datasets. The gridded datasets may be forecast data from Atmospheric, Ocean, or Climatology models. The GRIB files may have extensions like '.grib2', '.grib', 'grb', or '.gb' and in some cases, it may not even have a file name extension.

There are mainly two types of GRIB file formats i.e. `GRIB1` and `GRIB2`. Some info on the GRIB file format may be referred to here at [https://confluence.ecmwf.int](https://confluence.ecmwf.int/display/CKB/What+are+GRIB+files+and+how+can+I+read+them) and here at [https://weather.gc.ca](https://weather.gc.ca/grib/what_is_GRIB_e.html). 

## About `wgrib2`

`wgrib2` is one of the most versatile and fast tools available for reading and manipulating GRIB2 gridded datasets. For complete technical documentation, you may refer [https://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_doc/](https://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_doc/). The official documentation is available at [https://www.nco.ncep.noaa.gov](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/). Some of the frequently used options with examples are listed at [https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/tricks.wgrib2](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/tricks.wgrib2). Since this post is not about working with `wgrib2`, I will go ahead and install the package now.
 

## Install `wgirb2` on Linux

This guide is on how to install `wgrib2` on a Ubuntu OS. A guide for Windows users is given at the end of this article. 

Firstly, update the OS before proceeding further:

```bash
sudo apt-get update && apt-get upgrade
```

Install all necessary dependencies on Ubuntu and its derivatives:

```bash
sudo apt-get install -y build-essential libaec-dev zlib1g-dev libcurl4-openssl-dev libboost-dev curl wget zip unzip bzip2 gfortran gcc g++
```

If you use Redhat or its derivatives, use the following commands:

```bash
sudo groupinstall "Development Tools"
sudo dnf install gcc-gfortran csh perl
```

Make a working directory for compilation and move into it:

```bash
mkdir -p ~/Downloads/wgrib2
cd ~/Downloads/wgrib2
```

Download the latest source code and extract it to the current working directory:

```bash
wget -c ftp://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2.tgz
tar -xzvf wgrib2.tgz
```

Move to the `grib2` directory where all the necessary compilation files are located:

```bash
cd grib2
```

Compile the source code:

```bash
make
```

Note that it does not require `configure` as we do for many of the source compilations. If everything goes well, you should have compiled the `wgrib2` executable under the `wgrib2` directory. Check if it is properly compiled using the following command:

```bash
wgrib2/wgrib2 -config
```

which should print information like the below:

```bash
wgrib2 v3.1.1 4/2022  Wesley Ebisuzaki, Reinoud Bokhorst, John Howard, Jaakko Hyvätti, Dusan Jovic, Daniel Lee, Kristian Nilssen, Karl Pfeiffer, Pablo Romero, Manfred Schwarb, Gregor Schee, Arlindo da Silva, Niklas Sondell, Sam Trahan, George Trojan, Sergey Varlamov
    

Compiled on 14:16:59 Jan 10 2023

Netcdf package: 4.8.1 of Oct 31 2022 22:16:44 $ is installed
hdf5 package: system is installed
Jasper 2.0.33 is installed
mysql package is installed
regex package is installed
flush_mode determined by stat()
tigge package is installed
interpolation package is not installed, default vectors:
UGRD/VGRD VUCSH/VVCSH UFLX/VFLX UGUST/VGUST USTM/VSTM VDFUA/VDFVA MAXUW/MAXVW 
    UOGRD/VOGRD UICE/VICE U-GWD/V-GWD USSD/VSSD 
Geolocation library status (by search order)
  gctpc geolocation is enabled
  spherical geolocation is enabled
UDF package is not installed
version ftime=2
maximum number of arguments on command line: 10000
maximum number of -match,-not,-if, and -not_if options: 2000
maximum number of -match_fs,-not_fs,-if_fs, and -not_if_fs options: 2000
maximum number of -fgrep, -egrep, -fgrep_v, -egrep_v options: 200
RPN registers: 0..19
memory files: @mem:0, @mem:1 .. @mem:29
stdout buffer length: 100000
default decoding: g2clib emulation
g2clib decoders are not installed
Supported decoding: simple, complex, rle, ieee, png, jpeg2000
Supported encoding: simple, complex, ieee, jpeg2000
default WMO names: NCEP
C compiler: gcc
  CPPFLAGS= -Wall -Wmissing-prototypes -Wold-style-definition -Werror=format-security -ffast-math -O3 -DGFORTRAN 
OpenMP: control number of threads with environment variable OMP_NUM_THREADS
INT_MAX:   2147483647
ULONG_MAX: 18446744073709551615
```

If you wish to access `wgrib2` directly from the terminal anywhere as we do for other commands like `ls `, `cp`, `df` etc., copy the wgrib2 binary into the appropriate location as indicated below:

```bash
cp -rfv wgrib2/wgrib2 /usr/local/bin/wgrib2
```

If you wish to clean up the compilation directory, simply delete the working directory we have used so far with the following command:

```bash
rm -rfv ~/Downloads/wgrib2
```

That's it for Linux users.

## Install `wgrib2` on Windows
For Windows, there is a precompiled version is available at https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/Windows10/. Click on the directory that contains the latest version of wgrib2 and download all *.exe and *.dll files. Save all the downloaded files and run `wgrib2.exe`. That's it.

------
You can download this article from [here](https://wxguy.github.io/assets/downloads/pdfs/2023-01-10-install-wgrib2-on-linux.pdf) for free.

------

