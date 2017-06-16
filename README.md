AWS IoT Device SDK for Python
=============================

## Table of Contents
   - [Overview](#overview)
  -  [Installation](#installation)
  -  [Setting up AWS IoT components](#setting-up-aws-iot-components)
  -  [Features](#features)
  -  [Implementation](#implementation)
  -  [API Documentation](#api-documentation)
  -  [References](#references)

## Overview

This document provides instructions for installing and configuring the AWS IoT Device SDK for Python and run a PubSub program to illustrate MQTT architecture.

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

The auto-reconnect occurs with a progressive backoff, which follows this
mechanism for reconnect backoff time calculation:

This is t<sup>current</sup> = min (2<sup>n</sup> t<sup>base</sup>, t<sup>max</sup> )

where t<sup>current</sup>  is the current reconnect backoff time, t<sup>base</sup> is the base reconnect backoff time, t<sup>max</sup> is the maximum reconnect backoff time.n increases itreatively.

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

If the client is temporarily offline and disconnected due to network failure, publish requests will be added to an internal queue until the number of queued-up requests reaches the size limit of the queue. 

The following API is provided for configuration:
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureOfflinePublishQueueing(queueSize, dropBehavior)

After the queue is full, offline publish requests will be discarded or
replaced according to the configuration of the drop behavior:

    # Drop the oldest request in the queue
    AWSIoTPythonSDK.MQTTLib.DROP_OLDEST = 0
    # Drop the newest request in the queue
    AWSIoTPythonSDK.MQTTLib.DROP_NEWEST = 1

Let's say we configure the size of offlinePublishQueue to 5 and we
have 7 incoming offline publish requests.

In a ``DROP_OLDEST`` configuration:

	myClient.configureOfflinePublishQueueing(5, AWSIoTPythonSDK.MQTTLib.DROP_OLDEST);

The internal queue should be like this when the queue is just full:

    HEAD ['pub_req1', 'pub_req2', 'pub_req3', 'pub_req4', 'pub_req5']

When the 6th and the 7th publish requests are made offline, the internal
queue will be like this:

    HEAD ['pub_req3', 'pub_req4', 'pub_req5', 'pub_req6', 'pub_req7']

Because the queue is already full, the oldest requests ``pub_req1`` and
``pub_req2`` are discarded.

In a ``DROP_NEWEST`` configuration:

    myClient.configureOfflinePublishQueueing(5, AWSIoTPythonSDK.MQTTLib.DROP_NEWEST);

The internal queue should be like this when the queue is just full:

    HEAD ['pub_req1', 'pub_req2', 'pub_req3', 'pub_req4', 'pub_req5']

When the 6th and the 7th publish requests are made offline, the internal
queue will be like this:

    HEAD ['pub_req1', 'pub_req2', 'pub_req3', 'pub_req4', 'pub_req5']

Because the queue is already full, the newest requests ``pub_req6`` and
``pub_req7`` are discarded.

When the client is back online, connected, and resubscribed to all topics
it has previously subscribed to, the draining starts. All requests
in the offline publish queue will be resent at the configured draining
rate:
          			    
	AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureDrainingFrequency(frequencyInHz)
                                
If no ``configOfflinePublishQueue`` or ``configureDrainingFrequency`` is
called, the following default configuration for offline publish queueing
and draining will be performed on the initialization:

    offlinePublishQueueSize = 20
    dropBehavior = DROP_NEWEST
    drainingFrequency = 2Hz

Before the draining process is complete, any new publish request
within this time period will be added to the queue. Therefore, the draining rate
should be higher than the normal publish rate to avoid an endless
draining process after reconnect.

The disconnect event is detected based on PINGRESP MQTT
packet loss. Offline publish queueing will not be triggered until the
disconnect event is detected. Configuring a shorter keep-alive
interval allows the client to detect disconnects more quickly. Any QoS0
publish requests issued after the network failure and before the
detection of the PINGRESP loss will be lost.

***ConnectDisconnectTimeout***

Used to configure the time in seconds to wait for a CONNACK or a disconnect to complete.
	
     myAWSIoTMQTTClient.configureConnectDisconnectTimeout(10)
    

``timeoutSecond`` - Time in seconds to wait for a CONNACK or a disconnect to complete.

***MQTTOperationTimeout***

Used to configure the timeout in seconds for MQTT QoS 1 publish, subscribe and unsubscribe. Should be called before connect.

	myAWSIoTMQTTClient.configureMQTTOperationTimeout(5)
    
``timeoutSecond`` - Time in seconds to wait for a PUBACK/SUBACK/UNSUBACK.
     # AWSIoTMQTTClient connection configuration
	
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureAutoReconnectBackoffTime(1, 32, 20)
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureOfflinePublishQueueing(-1)  # Infinite offline Publish queueing
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureDrainingFrequency(2)  # Draining: 2 Hz
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureConnectDisconnectTimeout(10)  # 10 sec
    AWSIoTPythonSDK.MQTTLib.AWSIoTMQTTClient.configureMQTTOperationTimeout(5)  # 5 sec

### Implementation

The project has 2 scripts publisher.py and subscriber.py.

The publisher script publishes continuous messgaes to the topic ``temperature``.

The subscriber script subscribes to the same topic which the messages are being published to.

Change the following parameters accordingly in both Publisher and subscriber script:

    host = "endpoint.iot.region.amazonaws.com"
    rootCAPath = "path to your rootCA"
    certificatePath = "path to your X.509 certificate"
    privateKeyPath = "path to your private key"

**Make Sure to use your mobile data as HP network does not give access to the port 8883 for MQTT protocol**
### API Documentation

You can find the API documentation for the SDK [here](https://s3.amazonaws.com/aws-iot-device-sdk-python-docs/index.html)

### References

The base code was taken from [this repo](https://github.com/aws/aws-iot-device-sdk-python), and have been changed according to our requirements.
