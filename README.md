AWS IoT Device SDK for Python
=============================

## Table of Contents
   - [Overview](#overview)
  -  [Installation](#installation)
  -  [Setting up AWS IoT components](#setting-up-aws-iot-components)
  -  [Features](#features)
  -  [Examples](#examples)
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

 	  >>>  pip install AWSIoTPythonSDK

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
    

3) Link the certificate to both policy and the thing created in the registry.


[A detailed procedure for steps 1-3 are documented here ](http://docs.aws.amazon.com/iot/latest/developerguide/register-device.html)

### Features

**Progressive Reconnect Backoff**

When a non-client-side disconnect occurs, the SDK will reconnect automatically. The following APIs are provided for configuration:

AWSIoTMQTTClient connection configuration

**configureAutoReconnectBackoffTime**(baseReconnectQuietTimeSecond, maxReconnectQuietTimeSecond, stableConnectionTimeSecond)

baseReconnectQuietTimeSecond - The initial back off time to start with, in seconds. Should be less than the stableConnectionTime.

maxReconnectQuietTimeSecond - The maximum back off time, in seconds.

stableConnectionTimeSecond - The number of seconds for a connection to last to be considered as stable. Back off time will be reset to base once the connection is stable

The auto-reconnect occurs with a progressive backoff, which follows this
mechanism for reconnect backoff time calculation:

    sup:`current` = min(2\ :sup:`n` t\ :sup:`base`, t\ :sup:`max`)
    n^2
    k_{n+1}
    A = pi*r^{2}
where t\ :sup:`current` is the current reconnect backoff time, t\ :sup:`base` is the base
reconnect backoff time, t\ :sup:`max` is the maximum reconnect backoff time.

The reconnect backoff time will be doubled on disconnect and reconnect
attempt until it reaches the preconfigured maximum reconnect backoff
time. After the connection is stable for over the
``stableConnectionTime``, the reconnect backoff time will be reset to
the ``baseReconnectQuietTime``.

If no ``configureAutoReconnectBackoffTime`` is called, the following
default configuration for backoff timing will be performed on initialization:


    baseReconnectQuietTimeSecond = 1
    maxReconnectQuietTimeSecond = 32
    stableConnectionTimeSecond = 20

***OfflinePublishQueueing***

***DrainingFrequency***

***ConnectDisconnectTimeout***

***MQTTOperationTimeout***

     # AWSIoTMQTTClient connection configuration
	
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureAutoReconnectBackoffTime(1, 32, 20)
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureOfflinePublishQueueing(-1)  # Infinite offline Publish queueing
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureDrainingFrequency(2)  # Draining: 2 Hz
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureConnectDisconnectTimeout(10)  # 10 sec
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureMQTTOperationTimeout(5)  # 5 sec

### Examples
### API Documentation
### License
### Support

This document provides instructions for installing and configuringa
the AWS IoT Device SDK for Python. It includes examples demonstrating the
use of the SDK APIs.
The base code was taken from [this repo](https://github.com/aws/aws-iot-device-sdk-python), and have been changed according to our requirements.
