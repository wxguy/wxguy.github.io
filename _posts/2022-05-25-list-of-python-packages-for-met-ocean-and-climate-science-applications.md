---
title: List of Useful Python Packages for Atmospheric, Ocean and Climate Sciences
author: wxguy
date: 2022-05-25 21:10:00 +0530
categories: [Info, collections]
tags: [python,python-modules,python-packages,scientific-modules]
pin: false
toc: true
render_with_liquid: true
media_subpath: /assets/img/python_4_metoc/
image:
  path: python_sci_ecosystem.png
  alt: Sample Python Ecosystem for Scientific applications. (Credit:- https://www.datacamp.com/)
---

-------

Here is the list of Python modules, packages, and programs that I keep on referring to or visiting often. Most of the packages can be installed using `conda`, some using `pip`, and a few need to be installed using `setup.py` script. Descriptions of packages are taken from official sites or documentation. The list is exhaustive and hopes that some will benefit from my research while looking for the specific use case.

-------

## Integrated Development Environments or Text Editors

[PyCharm Community Edition](https://www.jetbrains.com/help/pycharm/installation-guide.html). It comes with a lot of goodies by default. Install it only if you have enough hardware resources.

[Sublime Text](https://www.sublimetext.com/download). Install Sublime Text with Anaconda and Tabnine plugins to make it a truly remarkable IDE.

[Spyder](https://docs.spyder-ide.org/current/installation.html). Highly recommended if you are working on Scientific applications. This is also the recommended IDE by the scientific community.

[ipython](https://ipython.org/). It is not a direct scientific Python package. IPython provides a rich architecture for interactive computing.

-------

## Core Packages

[numpy](https://numpy.org/). The fundamental package for scientific computing with Python. Core package around in which other scientific modules are built. 

[matplotlib](https://matplotlib.org/). A comprehensive library for creating static, animated, and interactive visualizations in Python. Matplotlib makes easy things easy and hard things possible. 

[scipy](https://scipy.org/). Fundamental algorithms for scientific computing in Python. It provides algorithms for optimization, integration, interpolation, eigenvalue problems, algebraic equations, differential equations, statistics, and many other classes of problems. 

[sympy](https://www.sympy.org/en/index.html). SymPy is a Python library for symbolic mathematics. It aims to become a full-featured computer algebra system (CAS) while keeping the code as simple as possible in order to be comprehensible and easily extensible. SymPy is written entirely in Python. 

-------

## Data Input/ Output

### NetCDF

[netcdf4-python](https://unidata.github.io/netcdf4-python/). A Python interface to the netCDF C library by unidata who is also the creator of netCDF itself. Most of the other easy-to-use NetCDF libraries are built on top of this library. 

[xarray](https://docs.xarray.dev/en/stable/). It is inspired by and borrows heavily from pandas, the popular data analysis package focused on labeled tabular data. It is particularly tailored to working with netCDF files, which were the source of xarray’s data model, and integrates tightly with dask for parallel computing. 

### GRIB 1/2

[pygrib](https://jswhit.github.io/pygrib/). A high-level Python interface to ECCODES library for GRIB file IO operations. If you are working on GRIB2 files, you can make use of this library. 

### HDF5

[h5py](https://docs.h5py.org ). Pythonic interface to the HDF5 binary data format.  It lets you store huge amounts of numerical data, and easily manipulate that data from NumPy.  

### Satellites

[satpy](https://satpy.readthedocs.io/en/stable/). It is a python library for reading, manipulating, and writing data from remote-sensing earth-observing satellite instruments. Satpy provides users with readers that convert geophysical parameters from various file formats to the common Xarray DataArray and Dataset classes for easier interoperability with other scientific Python libraries. 

### WxRadar

[wradlib](https://docs.wradlib.org/en/stable/). The ωradlib project has been initiated in order to facilitate the use of weather radar data as well as to provide a common platform for research on new algorithms. It supports various DWR radar formats available in the market.

[pycinrad](https://pycinrad.readthedocs.io/en/latest/index.html). PyCINRAD is an open-source weather radar library that supports the reading of all mainstream radar formats in China and provides some practical algorithms and visualizations. 

[pycwr](https://pycwr.readthedocs.io/en/latest/). The China Weather Radar Toolkit supports most of China's radar formats(WSR98D, CINRAD/SA/SB/CB, CINRAD/CC/CCJ, CINRAD/SC/CD).  

### Radiosondes

[pyigra](https://github.com/wxguy/PyIGRA/tree/master). The National Centres for Environmental Information provides a large global archive for vertical soundings also known as radiosondes. PyIGRA downloads and extracts data from the Integrated Global Radiosonde Archive data set and prepares them in a more handy data format. 

### Other Formats

[pygdal](https://gdal.org/api/python.html). GDAL, the Geospatial Data Abstraction Library, is an access and translator library for raster and vector geospatial data formats, often known as the swiss knife of geospatial. pygdal is a Python interface to the GDAL library. 

[argos](https://github.com/titusjan/argos). Argos is a GUI for viewing and exploring scientific data, written in Python and Qt. It has a plug-in architecture that allows it to be extended to read new data formats. At the moment plug-ins are included to read HDF-5, NetCDF-4, WAV, Exdir, NumPy binary files, and various image formats, but a plug-in could be written for any data that can be expressed as a Numpy array.

[siphon](https://www.unidata.ucar.edu/software/siphon/). Siphon is a collection of Python utilities for downloading data from remote data services. Much of Siphon’s current functionality focuses on access to data hosted on a THREDDS Data Server, although access to data hosted by other web technologies is being quickly added.  

-------

## Data Science, Machine Learning, and Artificial Intelligence

[pandas](https://pandas.pydata.org/). A fast, powerful, flexible, and easy-to-use open-source data analysis and manipulation tool, built on top of the Python programming language. This is the base package for working with a statistical dataset in Python. 

[seaborn](https://seaborn.pydata.org/). It is a Python data visualization library based on matplotlib. It provides a high-level interface for drawing attractive and informative statistical graphics. 

[theano](https://pypi.org/project/Theano/). Theano is a Python library that allows you to define, optimize, and efficiently evaluate mathematical expressions involving multi-dimensional arrays.

[tensorflow](https://www.tensorflow.org/learn). TensorFlow is an open-source library for fast numerical computing. This module makes it easy for beginners and experts to create machine learning models.

[keras](https://keras.io/). Keras is an API designed for human beings, not machines. Keras follows best practices for reducing cognitive load: it offers consistent & simple APIs, it minimizes the number of user actions required for common use cases, and it provides clear & actionable error messages.

[pytorch](https://pytorch.org/). PyTorch has a range of tools and libraries that support computer vision, machine learning, and natural language processing. The library is open-source and is based on the Torch library. 

[scikit-learn](https://scikit-learn.org/stable/index.html). It is a powerful Python library that was originally generated to serve the purpose of data modeling and building machine learning algorithms. It has a simple, engaging, and consistent interface that is exceptionally user-friendly, making it easy to use and share data.

[geopandas](https://geopandas.org/en/stable/). It is an open-source project to make working with geospatial data in python easier. GeoPandas extends the datatypes used by pandas to allow spatial operations on geometric types.  

[pytables](https://www.pytables.org/). PyTables is a package for managing hierarchical datasets and is designed to efficiently and easily cope with extremely large amounts of data.  

-------

## Atmospheric Science

[metpy](https://unidata.github.io/MetPy/latest/index.html). MetPy is a collection of tools in Python for reading, visualizing, and performing calculations with weather data. 

[wrf-python](https://wrf-python.readthedocs.io/en/latest/). A collection of diagnostic and interpolation routines for use with output from the Weather Research and Forecasting (WRF-ARW) Model.

[wrf_install](https://github.com/wxguy/wrf_install). automate the downloading & building process of the wrf model with all dependencies. 

[getgfs](https://github.com/jagoosw/getgfs). Extracts weather forecast variables from the NOAA GFS forecast with no obscure, platform-specific, dependencies. 

[pyart](https://arm-doe.github.io/pyart/). It is a Python module containing a collection of weather radar algorithms and utilities.  

[prob_meteogram](https://github.com/wxguy/prob_meteogram/tree/master). An intuitive probabilistc meteogram based on ensemble forecasts. 

[SkewTplus](https://skewtplus.readthedocs.io/en/latest/). SkewTplus provides useful tools for plotting and analyzing atmospheric sounding data. In particular, it provides useful classes to handle the awkward skew-x projection.

[tropycal](https://tropycal.github.io/tropycal/index.html). Python package intended to simplify the process of retrieving and analyzing tropical cyclone data, both for past storms and in real-time, and is geared towards the research and operational meteorology sectors. 

[aoslib](https://github.com/PyAOS/aoslib). aoslib is a Python library of standard atmospheric and oceanic sciences calculation routines. It exists mainly so we're all not writing our own routines to calculate potential temperature, isentropic potential vorticity, etc. 

[imdlib](https://imdlib.readthedocs.io/en/latest/). IMDLIB is a python package to download and handle binary gridded data from the India Meteorological Department (IMD). 

[verif](https://github.com/WFRT/verif). Verif is a command-line tool that lets you verify the quality of weather forecasts for point locations. It can also compare forecasts from different forecasting systems (that have different models, post-processing methods, etc). 

[eofs](https://ajdawson.github.io/eofs/latest/). eofs is a Python package for EOF analysis of spatial-temporal data. Using EOFs (empirical orthogonal functions) is a common technique to decompose a signal varying in time and space into a form that is easier to interpret in terms of spatial and temporal variance. 

[windspharm](https://ajdawson.github.io/windspharm/latest/). windspharm is a Python package for performing computations on global wind fields in spherical geometry. It provides a high-level interface for computations using spherical harmonics. 

[windrose](https://windrose.readthedocs.io/en/latest/index.html). A windrose, also known as a polar rose plot, is a special diagram for representing the distribution of meteorological data, typically wind speeds by class and direction. This is a simple module for the matplotlib python library, which requires NumPy for internal computation. 

[tephigrams](https://github.com/leifdenby/tephigram_python). Python package for plotting tephigrams.

[pymeteo](https://cwebster2.github.io/pyMeteo/). Skew-T/Log-P plotting routine, meteorology, and interfacing with CM1.  This package provides routines for plotting Skew-T/Log-P diagrams and working with CM1 output.  

[metar](https://python-metar.readthedocs.io/). metar is a python package for interpreting METAR and SPECI coded weather reports. 

[PythonMETAR](https://pypi.org/project/PythonMETAR/). Yet another Python package for interpreting METAR and SPECI weather reports. 

[xcape](https://xcape.readthedocs.io/en/latest/). xcape provides an efficient workflow for the calculation of parameters commonly used in meteorology, allowing for post-processing of 4D gridded model or observed data using xarray for parameters including Convective Available Potential Energy (CAPE) and Storm Relative Helicity (SRH). This is made possible by leveraging wrapped Fortran routines and NumPy and allows for process parallelization via Dask. 

[act](https://arm-doe.github.io/ACT/). Atmospheric Community Toolkit (ACT) is an open-source Python toolkit for working with atmospheric time-series datasets of varying dimensions. The toolkit is meant to have functions for every part of the scientific process; discovery, IO, quality control, corrections, retrievals, visualization, and analysis. 

[tcpyPI](https://github.com/dgilford/tcpyPI). tcpyPI, 'pyPI' for short, is a set of scripts and notebooks that compute and validate tropical cyclone (TC) potential intensity (PI) calculations in Python. 

[pyresample](https://pyresample.readthedocs.io/en/latest/). Pyresample is a python package for resampling geospatial image data. It is the primary method for resampling in the SatPy library, but can also be used as a standalone library. 

[gridded](https://noaa-orr-erd.github.io/gridded/). gridded is a single API for accessing/working with gridded model results on multiple grid types. 

[xESMF](https://xesmf.readthedocs.io/en/latest/). xESMF is a Python package for regrinding. It is powerful, easy to use, and fast. 

[uxarray](https://uxarray.readthedocs.io/en/latest/). Uxarray aims to provide xarray-styled functionality for unstructured grid datasets following ugrid conventions. 

[sharppy](https://sharppy.github.io/SHARPpy/index.html). SHARPpy is a collection of open-source sounding and hodograph analysis routines, a sounding plotting package, and an interactive, cross-platform application for analyzing real-time soundings all written in Python. It was developed to provide the atmospheric science community with a free and consistent source of sounding analysis routines. 

[pywrf](https://github.com/honnorat/pywrf). Python tools to launch and process WRF simulations. The code is a little old but still useful. 

[wrfplot](https://github.com/liamtill/wrfplot). Python script to plot various WRF-ARW outputs. Command-line driven by passing options. The script is written in Python 2.x but gives you insight on processing wrfout files. 

[new_wrf_plotting_functions](https://github.com/lmadaus/new_wrf_plotting_functions). Yet another collections of functions for plotting WRF data. 

[wrftools](https://github.com/keltonhalbert/wrftools). Functions for post-processing WRF output data. 

[wrf-aurorun](https://github.com/yyr/wrf-autorun). Scripts to automate a WRF model run. 



-------

## Data Visualisation

[cartopy](https://scitools.org.uk/cartopy/docs/latest/). This package is designed for geospatial data processing to produce maps and other geospatial data analyses.  It makes use of the powerful PROJ, NumPy, and Shapely libraries and includes a programmatic interface built on top of Matplotlib for the creation of publication-quality maps. 

[colormaps](https://pratiman-91.github.io/2021/08/10/Colormaps.html). Colormaps is a library of a collection of colormaps or color palettes for Python. It’s written in Python with matplotlib and NumPy as dependencies. You can use Colormaps to customize matplotlib plots.  

[cmocean](https://matplotlib.org/cmocean/). This package contains colormaps for commonly-used oceanographic variables. Most of the colormaps started from matplotlib colormaps, but have now been adjusted using the viscm tool to be perceptually uniform.

[psyplot](https://psyplot.github.io/psyplot/). psyplot is an open-source python project that mainly combines the plotting utilities of matplotlib and the data management of the xarray package. The main purpose is to have a framework that allows a fast, attractive, flexible, easily applicable, easily reproducible, and especially an interactive visualization of your data. 

[bokeh](https://bokeh.org/). Bokeh is an interactive visualization library that targets modern web browsers for presentation. Bokeh can help anyone who would like to quickly and easily connect powerful PyData tools to interactive plots, dashboards, and data applications. 

[pyferret](https://ferret.pmel.noaa.gov/Ferret/documentation/pyferret). Python interface to famous ferret visualisation tool for oceanographers. 

[gradspy(]https://cola.gmu.edu/grads/gadoc/python.html). Official Python extension of Grads plotting software. If the site is not reachable, please use `http` in the url.

[py3grads](https://github.com/meridionaljet/py3grads). Yet another Python 3 Interface to GrADS.

[pygmt](https://www.pygmt.org/latest/). PyGMT is a library for processing geospatial and geophysical data and making publication-quality maps and figures. It provides a Pythonic interface for the Generic Mapping Tools (GMT), a command-line program widely used in the Earth Sciences.

[geoviews](https://geoviews.org/). GeoViews is a Python library that makes it easy to explore and visualize geographical, meteorological, and oceanographic datasets, such as those used in weather, climate, and remote sensing research. 

[geocat-viz](https://geocat-viz.readthedocs.io/en/latest/). The GeoCAT-viz repo contains tools to help plot data, including convenience and plotting functions that are used to facilitate plotting geosciences data with Matplotlib, Cartopy, and possibly other Python ecosystems plotting packages. 

[pylustrator](https://pylustrator.readthedocs.io/en/latest/). Pylustrator is software to prepare your figures for publication in a reproducible way. This means you receive a figure representing your data alongside a generated code file that can exactly reproduce the figure as you put them in the publication, without the need to readjust things in external programs. 

[pyproj](https://pyproj4.github.io/pyproj/stable/). Python interface to PROJ (cartographic projections and coordinate transformations library). Most of the Plotting packages which require geo-reference uses this package. 

-------

## Climate and Forecast (CF) Conventions

[cftime](https://unidata.github.io/cftime/). Python library for decoding time units and variable values in a netCDF file conforming to the Climate and Forecasting (CF) netCDF conventions. 

[cf-plot](https://ajheaps.github.io/cf-plot/). A set of Python routines for making the common contour, vector, and line plots that climate researchers use.  

[cf-python](https://ncas-cms.github.io/cf-python/). An Earth Science data analysis library that is built on a complete implementation of the CF data model. 

[cfdm](https://ncas-cms.github.io/cfdm/). This library implements the data model of the CF (Climate and Forecast) metadata conventions [https://cfconventions.org](https://cfconventions.org) and so should be able to represent and manipulate all existing and conceivable CF-compliant datasets. 

[cf-checker](https://github.com/cedadev/cf-checker). The CF Checker is a utility that checks the contents of a NetCDF file and complies with the Climate and Forecasts (CF) Metadata Convention. 

[cf_xarray](https://cf-xarray.readthedocs.io/en/latest/). cf_xarray mainly provides an accessor (DataArray.cf or Dataset.cf) that allows you to interpret Climate and Forecast metadata convention attributes present on xarray objects. 

-------

## Climate Science

[climetlab](https://climetlab.readthedocs.io/en/latest/). CliMetLab is a Python package aiming at simplifying access to climate and meteorological datasets, allowing users to focus on science instead of technical issues such as data access and data formats. 

[aospy](https://aospy.readthedocs.io/en/stable/). aospy (pronounced A - O - S - py) is an open-source Python package for automating your computations that use gridded climate and weather data (namely data stored as netCDF files) and the management of the results of those computations. 

[xclim](https://xclim.readthedocs.io/en/stable/). xclim is a library of functions to compute climate indices from observations or model simulations. Its objective is to make it as simple as possible for users to compute indices from large climate datasets and for scientists to write new indices with very little boilerplate. 

[climate-indices](https://climate-indices.readthedocs.io/en/latest/). This project contains Python implementations of various climate index algorithms which provide a geographical and temporal picture of the severity of precipitation and temperature anomalies useful for climate monitoring and research. 

-------

## Oceanographic Science

[verde](https://www.fatiando.org/verde/latest/). Verde is a Python library for processing spatial data (bathymetry, geophysics surveys, etc) and interpolating it on regular grids (i.e., gridding).  

[oceanspy](https://oceanspy.readthedocs.io/en/latest/). It is a Python package to facilitate ocean model data analysis and visualization. 

[sea-py](https://pyoceans.github.io/sea-py/). Python Tools for Oceanographic Analysis. It is not a package by itself. It is a webpage with a collection of useful oceanography-related analysis tools and tutorials. 

[xmitgcm](https://xmitgcm.readthedocs.io/en/latest/). It is a python package for reading MITgcm binary MDS files into xarray data structures. By storing data in dask arrays, xmitgcm enables parallel, out-of-core analysis of MITgcm output data. 

[octant](https://octant-docs.readthedocs.io/en/latest/). A Python library used for post-processing the output of PMCTRACK - vorticity-based cyclone tracking algorithm. 

[pyroms](https://github.com/ESMG/pyroms). Pyroms is a collection of tools to process input and output files from the Regional Ocean Modeling System, ROMS. It was originally started by Rob Hetland as a google code project, then he morphed it into octant, also at google code. Frederic Castruccio then created a fork and renamed it back to pyroms. 

[ctd](https://pypi.org/project/ctd/). Tools to load hydrographic data as pandas DataFrame with some handy methods for data pre-processing and analysis. This module can load SeaBird CTD (CNV), Sippican XBT (EDF), and Falmouth CTD (ASCII) formats. 

[seawater](https://pypi.org/project/seawater/ ). This is a Python library for calculating the properties of seawater. The package uses the formulas from Unesco’s joint panel on oceanographic tables and standards, UNESCO 1981 and UNESCO 1983 (EOS-80). 

[gsw](https://pypi.org/project/gsw/). This Python implementation of the Thermodynamic Equation of Seawater 2010 (TEOS-10) is based primarily on NumPy ufunc wrappers of the GSW-C implementation.  

[ttide_py](https://github.com/moflaher/ttide_py).  Direct conversion of T_Tide to Python. 

[pytides](https://github.com/sam-cox/pytides). Pytides is a small Python package for the analysis and prediction of tides. Pytides can be used to extrapolate the tidal behavior at a given location from its previous behaviour.  

[pywavelets](https://pywavelets.readthedocs.io/en/latest/). It is open-source wavelet transform software for Python. It combines a simple high-level interface with low-level C and Cython performance.  

[pywafo](https://github.com/wafo-project/pywafo). It is WAFO toolbox Python routines for statistical analysis and simulation of random waves and random loads. 

[okean](https://github.com/martalmeida/okean).  ocean modeling and analysis tools. But unfortunately, there is no documentation provided on the site. 

[pymc](https://docs.pymc.io/en/v3/). PyMC3 allows you to write down models using an intuitive syntax to describe a data-generating process. 

[pygridgen](https://pygridgen.github.io/pygridgen/).  A Python interface to Pavel Sakov’s gridgen library. 

[mixsea](https://mixsea.readthedocs.io/en/v0.1.1/). Ocean mixing parameterizations in python. 

[xgcm](https://xgcm.readthedocs.io/en/latest/). xgcm is a python package for working with the datasets produced by numerical General Circulation Models (GCMs) and similar gridded datasets that are amenable to finite volume analysis. 


-------

## Geo Spatial

[qgis](https://qgis.org/en/site/). QGIS is a free and open-source cross-platform desktop geographic information system application that supports viewing, editing, printing, and analysis of geospatial data. It also has a Python API to extend its ability.  

[pysal](https://pysal.org/pysal/).  Python spatial analysis library is an open-source cross-platform library for geospatial data science with an emphasis on geospatial vector data written in Python.  

[geopy](https://geopy.readthedocs.io/en/stable/). It is a Python client for several popular geocoding web services. geopy makes it easy for Python developers to locate the coordinates of addresses, cities, countries, and landmarks across the globe using third-party geocoders and other data sources. 

-------

## Astrophysics, Seismology Etc.,

[argopy](https://argopy.readthedocs.io/en/latest/). argopy is a python library dedicated to Argo data access, manipulation, and visualisation for standard users as well as Argo experts. 

[yt](https://yt-project.org/). yt is a tool for querying, analyzing, and visualizing objects or regions of interest to identify emergent properties in data available in a variety of real-world research data formats. Initially developed for use by professional astronomers, yt can be applied in a variety of domains including astrophysics, seismology, nuclear engineering, molecular dynamics, and oceanography. 

[sunpy](https://sunpy.org/). SunPy is a Python-based software library that provides tools for performing research using direct observations of the Sun and Heliosphere. 


------
You can download this article from [here](https://wxguy.github.io/assets/downloads/pdfs/2022-05-25-list-of-python-packages-for-met-ocean-and-climate-science-applications.pdf) for free.

------

