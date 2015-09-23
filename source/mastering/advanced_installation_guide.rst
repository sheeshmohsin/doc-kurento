%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
Kurento Media Server Advanced Installation guide
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

Kurento Media Server Configuration
==================================

The main KMS configuration file is located in
``/etc/kurento/kurento.conf.json``. After a fresh installation this file is the
following:

.. sourcecode:: js

   {
     "mediaServer" : {
       "resources": {
       //  //Resources usage limit for raising an exception when an object creation is attempted
       //  "exceptionLimit": "0.8",
       //  // Resources usage limit for restarting the server when no objects are alive
       //  "killLimit": "0.7",
           // Garbage collector period in seconds
           "garbageCollectorPeriod": 240
       },
       "net" : {
         // Uncomment just one of them
         /*
         "rabbitmq": {
           "address" : "127.0.0.1",
           "port" : 5672,
           "username" : "guest",
           "password" : "guest",
           "vhost" : "/"
         }
         */
         "websocket": {
           "port": 8888,
           //"secure": {
           //  "port": 8433,
           //  "certificate": "defaultCertificate.pem",
           //  "password": ""
           //},
           //"registrar": {
           //  "address": "ws://localhost:9090",
           //  "localAddress": "localhost"
           //},
           "path": "kurento",
           "threads": 10
         }
       }
     }
   }

As of Kurento Media Server version 6, in addition to this general configuration
file, the specific features of KMS are tuned as individual modules. Each of
these modules has its own configuration file:

* ``/etc/kurento/modules/kurento/MediaElement.conf.ini``: Generic parameters
  for Media Elements.

* ``/etc/kurento/modules/kurento/SdpEndpoint.conf.ini``: Audio/video
  parameters for *SdpEndpoints* (i.e. *WebRtcEndpoint* and *RtpEndpoint*).

* ``/etc/kurento/modules/kurento/WebRtcEndpoint.conf.ini``: Specific
  parameters for *WebRtcEndpoint*.

* ``/etc/kurento/modules/kurento/HttpEndpoint.conf.ini``: Specific parameters
  for *HttpEndpoint*.


Verifying Kurento Media Server installation
===========================================

Kurento Media Server Process
----------------------------

To verify that KMS is up and running use the command:

.. sourcecode:: none

    ps -ef | grep kurento-media-server

The output should include the ``kurento-media-server`` process:

.. sourcecode:: none

   nobody    1270     1  0 08:52 ?        00:01:00 /usr/bin/kurento-media-server


WebSocket Port
--------------

Unless configured otherwise, KMS will open the port **8888** to receive requests
and send responses to/from by means of the
:doc:`Kurento Protocol<kurento_protocol>`. To verify if this port is listening
execute the following command:

.. sourcecode:: none

    sudo netstat -putan | grep kurento

The output should be similar to the following:

.. sourcecode:: none

   tcp6    0    0 :::8888    :::*    LISTEN    1270/kurento-media-server


Kurento Media Server Log
------------------------

Kurento Media Server logs file are stored in the folder
``/var/log/kurento-media-server/``. The content of this folder is as follows:

* ``media-server_<timestamp>.<log_number>.<kms_pid>.log``: Current log for
  Kurento Media Server

* ``media-server_error.log``: Third-party errors

* ``logs``: Folder that contains the KMS rotated logs


When KMS starts correctly, this trace is written in the log file:

.. sourcecode:: none

   [time] [0x10b2f880] [info]    KurentoMediaServer main.cpp:239 main() Mediaserver started

