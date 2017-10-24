---
layout: post
title:  Building openalpr from source in ubuntu 16.04
---

# How-To:
## 1. Get the major code source:
* git clone the openalpr master source from [github](https://github.com/openalpr/openalpr)
* git clone log4cplus v1.2.0 from [source](https://sourceforge.net/projects/log4cplus/files/log4cplus-stable/1.2.0/)
* grab Leptonic v1.74.4 from [site](http://www.leptonica.org/source/leptonica-1.74.4.tar.gz)
* git clone tesseract v4.00.00alpha (2017/10) from [github](https://github.com/tesseract-ocr/tesseract/)
## 2. Building 
* NOTE: install all above building results to a specified directory, e.g., $HOME/alpr/local (pls mkdir it firstly)
* log4cplus:
  - CXXFLAGS+="-std=c++11" ./configure --prefix=$HOME/alpr/local --enable-static=yes --enable-shared=no
  - make -j 4 && make install
* Leptonic:
  - CXXFLAGS+="-std=c++11" ./configure --prefix=$HOME/alpr/local
  - make -j 4 && make install
* Tesseract:
  - ./configure --prefix=$HOME/alpr/local --with-extra-libraries=$HOME/alpr/local/lib
  - make -j4 && make install


## 3. Issues

## 3. Refee: 
1. [openalpr from source](https://github.com/openalpr/openalpr/wiki/Compilation-instructions-(Ubuntu-Linux))
2. [tesseract from source](https://github.com/tesseract-ocr/tesseract/wiki/Compiling#Linux)
