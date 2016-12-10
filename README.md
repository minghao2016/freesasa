FreeSASA
========

[![DOI](https://zenodo.org/badge/18467/mittinatten/freesasa.svg)]
(https://zenodo.org/badge/latestdoi/18467/mittinatten/freesasa)
[![Build Status](https://travis-ci.org/mittinatten/freesasa.svg?branch=master)]
(https://travis-ci.org/mittinatten/freesasa)
[![Coverage Status](https://coveralls.io/repos/github/mittinatten/freesasa/badge.svg?branch=master)](https://coveralls.io/github/mittinatten/freesasa?branch=master)

C-library for calculating Solvent Accessible Surface Areas.

License: MIT (see file LICENSE). Copyright: Simon Mitternacht 2013-2016.

FreeSASA is a C library and command line tool for calculating Solvent
Accessible Surface Area (SASA) of biomolecules. It is designed to be
very simple to use with defaults, but allows customization of all
parameters of the calculation and provides a few different tools to
analyze the results as well. Python bindings are also included in the
repository.

The library includes both the algorithm by Lee & Richards and that by
Shrake & Rupley. Verification has been done by comparing the results
of the two calculations and by visual inspection of the surfaces found
by them (and comparing with analytic results in the two-atom
case). Both can be parameterized to arbitrary precision, and for high
resolution versions of the algorithms, the calculations give identical
results.

FreeSASA assigns a radius and a class to each atom. The atomic radii
are by default those defined by Tsai et al. ([JMB 1999, 290:
253](http://www.ncbi.nlm.nih.gov/pubmed/10388571)) for standard amino
acids and nucleic acids, and the van der Waals radius of the element
for other atoms. Each atom is also assigned to a class. The default
classes are `Apolar` (carbon) and `Polar` (all other elements). The
program outputs the total SASA and the area of these two classes, if a
protein has more than one chain the contribution of each chain is also
included in the output.

Users can provide their own atomic radii and classes, either via
configuration files or via the API. The input format for configuration
files is described in the [online
documentation](http://freesasa.github.io/doxygen/Config-file.html),
and the `share/` directory contains some sample configurations,
including one for the NACCESS parameters ([Hubbard & Thornton
1993](http://www.bioinf.manchester.ac.uk/naccess/)).

Compiling and installing
------------------------

FreeSASA can be compiled and installed using the following

    ./configure
    make && make install

NB: If the source is downloaded from the git repository the
configure-script needs to be set up first using `autoreconf -i`. Users
who don't have autotools installed, can download a tarball that
includes the autogenerated scripts from http://freesasa.github.io/ or
from the latest
[GitHub-release](https://github.com/mittinatten/freesasa/releases).

The above commands build and install the command line tool `freesasa`
(built in `src/`), the command

    freesasa -h

gives an overview of options. To run a calculation from PDB-file input
using the defaults, simply type

    freesasa <pdb-file>

`make install` also installs the header `freesasa.h` and the library
`libfreesasa`. If the configure script is called with the option
`--enable-python-bindings`, the Python module is also built and 
installed.

The configuration script can be customized with general options:
* `--enable-python-bindings` builds Python bindings, requires Cython
    0.21 or higher. On some platforms the C library needs to be
    compiled with `CFLAGS=-fPIC` to allow it to be linked to the
    Python module.
* `--with-python=<python>` specifies which python binary to use
* `--disable-threads` build without multithreaded calculations
* `--enable-doxygen` activates building of Doxygen documentation
* `--disable-json` build without support for JSON output.
* `--disable-xml` build without support for XML output.

And some options for developers:
* `--enable-check` enables unit-testing using the Check framework
* `--enable-gcov` adds compiler flags for measuring coverage of tests
    using gcov
* `--enable-parser-generator` rebuild parser/lexer source from
    Flex/Bison sources (the autogenerated code is included in the
    repository, so no need to do this if you are not going to change
    the parser).

Documentation
-------------

Enabling Doxygen builds a [full reference
manual](http://freesasa.github.io/doxygen/), documenting both CLI and
API in the folder `doc/html/doxygen/`, also available on the web at
http://freesasa.github.io/.

After building the package, calling
 
    freesasa -h
    
explains how the commandline tool can be used.

Compatibility and dependencies
------------------------------

The program has been tested successfully with several versions of GNU
C Compiler and Clang/LLVM. Building the library only requires standard
C and GNU libraries. Optional dependencies are
[json-c](https://github.com/json-c/json-c) and
[libxml2](http://xmlsoft.org/), users who want to build the library
without either can configure with `--disable-json` and `--disable-xml`
respectively.

Developers who want to do testing need to install the Check unit
testing framework. Building the full reference manual requires Doxygen
(version > 1.8.8). Building the Python bindings requires
Cython. Changing the selection parser and lexer requires Flex and
Bison. These build options, which add extra dependencies, are disabled
by default to simplify installation for users only interested in the
command line tool and and/or C Library.

Citing FreeSASA
---------------

FreeSASA can be cited using the following publication

* Simon Mitternacht (2016) FreeSASA: An open source C library for
  solvent accessible surface area calculations. _F1000Research_
  5:189. (doi:
  [10.12688/f1000research.7931.1](http://dx.doi.org/10.12688/f1000research.7931.1))

The [DOI numbers from Zenodo](https://zenodo.org/badge/latestdoi/18467/mittinatten/freesasa)
can be used to cite a specific version of FreeSASA.
