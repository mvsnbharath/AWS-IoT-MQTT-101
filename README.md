AWS IoT Device SDK for Python
=============================

## Table of Contents
   - [Overview](#overview)
  -  [Installation](#installation)
  -  [Use the SDK](#se-the-sdk)
  -  [Key Features](#)
  -  [Examples](#)
  -  [API Documentation](#api-documentation)
  -  [License](#license)
  -  [Support](#support)


## Overview

This document provides instructions for installing and configuring the AWS IoT Device SDK for Python. 

[This repo provides instructions for installing and configuring the AWS IoT Device SDK for Python](https://github.com/aws/aws-iot-device-sdk-python)


## Installation

Minimum Requirements
____________________

-  Python 2.7+ or Python 3.3+
-  OpenSSL version 1.0.1+ (TLS version 1.2) compiled with the Python executable for
   X.509 certificate-based mutual authentication

   To check your version of OpenSSL, use the following command in a Python interpreter:

   .. code-block:: python

       >>> import ssl
       >>> ssl.OPENSSL_VERSION

Install from pip
________________


.. code-block:: sh

    pip install AWSIoTPythonSDK

Build from source
_________________


.. code-block:: sh

    git clone https://github.com/aws/aws-iot-device-sdk-python.git
    cd aws-iot-device-sdk-python
    python setup.py install

Download the zip file
_____________________


The SDK zip file is available `here <https://s3.amazonaws.com/aws-iot-device-sdk-python/aws-iot-device-sdk-python-latest.zip>`__. Unzip the package and install the SDK like this:

.. code-block:: python

    python setup.py install

## Use the SDK
### Key Features
### Examples
### API Documentation
### License
### Support




This document provides instructions for installing and configuringa
the AWS IoT Device SDK for Python. It includes examples demonstrating the
use of the SDK APIs.
The base code was taken from [this repo](https://github.com/aws/aws-iot-device-sdk-python), and have been changed according to our requirements.
