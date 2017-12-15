# Intel&reg; QuickAssist Technology (QAT) Async Mode Nginx

## Table of Contents

- [Introduction](#introduction)
- [Licensing](#licensing)
- [Features](#features)
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Additional Information](#additional-information)
- [Limitations](#limitations)
- [Installation Instructions](#installation-instructions)
    - [Build Intel&reg; QuickAssist Technology Driver](#build-intel-quickassist-technology-driver)
    - [Install Async Mode Nginx](#install-async-mode-nginx)
- [Intended Audience](#intended-audience)
- [Legal](#legal)

## Introduction

Nginx\* [engine x] is an HTTP and reverse proxy server, a mail proxy server,
and a generic TCP/UDP proxy server, originally written by Igor Sysoev.
This project provides an extended Nginx working with asynchronous mode OpenSSL\*.
With Intel&reg; QuickAssist Technology(QAT) acceleration, the asynchronous mode Nginx
can provide significantly performance improvement. Besides that Async Mode Nginx
improves Nginx with new SSL engine framework which provides flexibility and usability
of integrating Intel&reg; QuickAssist Technology.

## Licensing

The Licensing of the files within this project is split as follows:

Intel&reg; Quickassist Technology (QAT) Async Mode Nginx - BSD License. Please 
see the `LICENSE` file contained in the top level folder. Further details can 
be found in the file headers of the relevant files.

## Features

* Asynchronous Mode in SSL/TLS processing

## Hardware Requirements

Async Mode Nginx supports Crypto offload to the following acceleration devices:

* Intel&reg; C62X Series Chipset
* [Intel&reg; Communications Chipset 8925 to 8955 Series][1]

[1]:https://www.intel.com/content/www/us/en/ethernet-products/gigabit-server-adapters/quickassist-adapter-8950-brief.html

## Software Requirements

This release was validated on the following:

* OpenSSL-1.1.0f
* QAT engine v0.5.30

## Additional Information

Async Mode Nginx SSL engine framework provides new directives:

**Directives**
```bash
Syntax:     ssl_asynch on | off;
Default:    ssl_asynch off;
Context:    http, server

    Enables SSL/TLS asynchronous mode
```
**Example**

file: conf/nginx.conf

```bash
    http {
        ssl_asynch  on;
        server {
            ...
            }
        }
    }
```

```bash
    http {
        server {
            ssl_asynch  on;
            }
        }
    }
```

## Limitations

* Nginx support `reload` operation, when QAT hardware is involved for crypto 
  offloading, user should enure that there are enough number of qat instances.
  For example, the available qat instance number should be 2x than Nginx worker
  process number.

## Installation Instructions

### Build Intel&reg; QuickAssist Technology Driver

Please follow the instructions contained in:

**For Intel&reg; C62X Series Chipset:**
Intel&reg; QuickAssist Technology Software for Linux\* - Getting Started Guide - HW version 1.7 (336212)

**For Intel&reg; Communications Chipset 89XX Series:**
Intel&reg; Communications Chipset 89xx Series Software for Linux\* - Getting
Started Guide (330750)

These instructions can be found on the 01.org website in the following section:

[Intel&reg; Quickassist Technology][1]

[1]:https://01.org/packet-processing/intel%C2%AE-quickassist-technology-drivers-and-patches

### Build OpenSSL\*

**For OpenSSL:**
Please refer to [OpenSSL][2]

[2]: https://github.com/openssl/openssl 

### Build QAT_engine

**For QAT_engine:**
Please refer to [QAT_engine][3]

[3]: https://github.com/intel/QAT_Engine

### Install QATzip

**Set the following environmental variables:**

`NGINX_INSTALL_DIR` is the directory where nginx will be installed to

`OPENSSL_LIB` is the directory where the openssl has been installed to

**Configure nginx for compilation:**

```bash
    ./configure \
        --prefix=$NGINX_INSTALL_DIR \
        --with-http_ssl_module \
        --with-cc-opt="-DNGX_SECURE_MEM -I$OPENSSL_LIB/include -Wno-error=deprecated-declarations" \
        --with-ld-opt="-Wl,-rpath=$OPENSSL_LIB/lib -L$OPENSSL_LIB/lib"
```

**Compile and Install:**

```bash
    make
    make install
```

## Intended Audience

The target audience may be software developers, test and validation engineers, 
system integrators, end users and consumers for Async Mode Nginx integrated 
Intel&reg; Quick Assist Technology.

## Legal

Intel&reg; disclaims all express and implied warranties, including without
limitation, the implied warranties of merchantability, fitness for a
particular purpose, and non-infringement, as well as any warranty arising
from course of performance, course of dealing, or usage in trade.

This document contains information on products, services and/or processes in
development.  All information provided here is subject to change without
notice. Contact your Intel&reg; representative to obtain the latest forecast
, schedule, specifications and roadmaps.

The products and services described may contain defects or errors known as
errata which may cause deviations from published specifications. Current
characterized errata are available on request.

Copies of documents which have an order number and are referenced in this
document may be obtained by calling 1-800-548-4725 or by visiting
www.intel.com/design/literature.htm.

Intel, the Intel logo are trademarks of Intel Corporation in the U.S.
and/or other countries.

\*Other names and brands may be claimed as the property of others

