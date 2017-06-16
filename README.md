AWS IoT Device SDK for Python
=============================

## Table of Contents
   - [Overview](#overview)
  -  [Installation](#installation)
  -  [Setting up AWS IoT components](#setting-up-aws-iot-components)
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


-  Python 2.7+ or Python 3.3+
-  OpenSSL version 1.0.1+ (TLS version 1.2) compiled with the Python executable for
   X.509 certificate-based mutual authentication

   To check your version of OpenSSL, use the following command in a Python interpreter:

       >>> import ssl
       >>> ssl.OPENSSL_VERSION

Install from pip

    pip install AWSIoTPythonSDK

## Setting up AWS IoT components

##### Thing Registry

1) The first step would be to crate a thing in AWS IoT console.

##### Certificates

2) We need to create at following one set of certificates. The process goes like this: 

Download all the certificates: 
    
	i) public key  
    ii) private key 
	iii) X.509  
	iv) aws-iot-rootCA
    

3) Link the certificate to each thing in the registry,


[A detailed procedure for steps 1-3 are documented here ](http://docs.aws.amazon.com/iot/latest/developerguide/register-device.html)

### Key Features


### Examples
### API Documentation
### License
### Support




This document provides instructions for installing and configuringa
the AWS IoT Device SDK for Python. It includes examples demonstrating the
use of the SDK APIs.
The base code was taken from [this repo](https://github.com/aws/aws-iot-device-sdk-python), and have been changed according to our requirements.
