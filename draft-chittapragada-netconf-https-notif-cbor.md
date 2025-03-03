---
title: "CBOR Encoding for HTTPS-based Transport for YANG Notifications"
abbrev: "https-notif-cbor"
category: std

docname: draft-chittapragada-netconf-https-notif-cbor-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: ""
workgroup: "Network Configuration"
keyword:
 - cbor
 - https
 - yang

venue:
  group: "Network Configuration"
  type: ""
  mail: "netconf@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/netconf/"
  github: "MeherRushi/draft-chittapragada-netconf-https-notif-cbor"
  latest: "https://MeherRushi.github.io/draft-chittapragada-netconf-https-notif-cbor/draft-chittapragada-netconf-https-notif-cbor.html"

author:
 -
    ins: B. M. Chittapragada
    name: Bharadwaja Meherrushi Chittapragada
    email: meherrushi2@gmail.com
 -
    ins: S. Bhat
    name: Siddharth Bhat
    email: siddharth.bhat10@gmail.com
 -
    ins: V. T. Rao
    name: Vartika T Rao
    email: vartikatrao@gmail.com
 -
    ins: H. Arshad
    name: Hayyan Arshad
    email: hayyanhamnah@gmail.com
 -
    ins: M. P. Tahiliani
    name: Mohit P. Tahiliani
    org: National Institute of Technology Karnataka, Surathkal
    email: tahiliani@nitk.edu.in

normative:
 I-D.draft-ietf-netconf-https-notif-15:
 RFC8949:
 RFC9254:

informative:
 RFC3553:


--- abstract

This document extends {{!I-D.draft-ietf-netconf-https-notif-15}} by introducing CBOR encoding for YANG notifications over HTTPS in addition to the existing JSON and XML encoding schemes.


--- middle

# Introduction

This document introduces a CBOR encoding scheme for event notifications over HTTPS by using the framework proposed in {{!I-D.draft-ietf-netconf-https-notif-15}} which supports transfer of YANG notifications over HTTPS using JSON and XML encoding schemes.


In {{!I-D.draft-ietf-netconf-https-notif-15}}, the capabilities HTTP-target resource allows a publisher to retrieve supported encoding formats via a GET request, while the relay-notification resource enables the publisher to send YANG notifications via POST requests. These requests and responses use different content types based on the selected encoding scheme. This document defines support for using CBOR encoding as mentioned in section 1 of {{!I-D.draft-ietf-netconf-https-notif-15}}

CBOR offers an efficient and compact representation of YANG notifications.

Examples of the GET and POST request and reply encoded in CBOR are also provided.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Terminology

This document uses the following terms defined in Section 2,3 and 4 of {{!I-D.draft-ietf-netconf-https-notif-15}}:

   - Capabilities Resource

   - Relay-Notification

   - Event Notification

The following term(s) are defined in Subscription to YANG Notifications {{!RFC8639}}:

   - Publisher

   - Receiver

   - Subscribed Notifications

The following term(s) are defined in Encoding of Data Modeled with YANG in the Concise Binary Object Representation (CBOR) {{!RFC9254}}:

   - Diagnostic Notifications

   - YANG Schema Item iDentifier (or "YANG SID" or simply "SID"): 63-bit unsigned integer used to identify different YANG items.



# CBOR Encoding of the notification(s)

YANG notifications can be encoded in CBOR using Names or SIDs in keys. Notifications encoded using names is similar to JSON encoding as defined in Section 3.4 and 4.3 of {{!I-D.draft-ietf-netconf-https-notif-15}}. Notification encoded using YANG-SIDs replaces the names of the keys of the CBOR encoded message with a 63 bit unsigned integer.  In this case, the term 'SID' is defined in Section 3.2 of {{!RFC9254}}, and the keys of the encoded data use SID value as mentioned in 4.3.2 of this document.

## Capabilities Request

The publisher sends a request to the receiver to learn its capabilities. In the below example, the “Accept” states that the publisher wants to receive the capabilities response in CBOR but if not supported then in XML or JSON in that order.

~~~ http-request
GET /some/path/capabilities HTTP/1.1
   Host: example.com
   Accept: application/cbor, application/xml;0.9, application/json;q=0.5
~~~

## Capabilities Response

 If the receiver is able to reply using “application/cbor” and assuming it is capable of receiving JSON, XML and CBOR encoded messages the response would look like this

### CBOR using names as keys

Diagnostic Notation:

~~~ http-message
   HTTP/1.1 200 OK
   Date: Tue, 4 March 2025 20:33:30 GMT
   Server: example-server
   Cache-Control: no-cache
   Content-Type: application/json
   {
   "receiver-capabilities": {
     "receiver-capability": [
       "urn:ietf:capability:https-notif-receiver:encoding:json",
       "urn:ietf:capability:https-notif-receiver:encoding:xml",
       "urn:ietf:capability:https-notif-receiver:encoding:cbor"
        ]
      }
   }
~~~

CBOR Encoding:

~~~
A1                                      # map(1)
   75                                   # text(21)
      72656365697665722D6361706162696C6974696573 # "receiver-capabilities"
   A1                                   # map(1)
      73                                # text(19)
         72656365697665722D6361706162696C697479 # "receiver-capability"
      83                                # array(3)
         78 36                          # text(54)
            75726E3A696574663A6361706162696C6974793A68747470732D6E6F7469662D72656365697665723A656E636F64696E673A6A736F6E # "urn:ietf:capability:https-notif-receiver:encoding:json"
         78 35                          # text(53)
            75726E3A696574663A6361706162696C6974793A68747470732D6E6F7469662D72656365697665723A656E636F64696E673A786D6C # "urn:ietf:capability:https-notif-receiver:encoding:xml"
         78 36                          # text(54)
            75726E3A696574663A6361706162696C6974793A68747470732D6E6F7469662D72656365697665723A656E636F64696E673A63626F72 # "urn:ietf:capability:https-notif-receiver:encoding:cbor"
~~~


##  Relay Notification request

The publisher sends an HTTP POST request to the "relay-notification" resource on the receiver with the "Content-Type" header set to either "application/cbor" in case the receiver is CBOR capable and a body containing the notification encoded in CBOR.

### CBOR encoding using names as keys

~~~ http-request
POST /some/path/relay-notification HTTP/1.1
   Host: example.com
   Content-Type: application/cbor
~~~

Diagnostic notation:

~~~ http-response
   {
     "ietf-https-notif:notification": {
       "eventTime": "2013-12-21T00:01:00Z",
       "example-mod:event" : {
         "event-class" : "fault",
         "reporting-entity" : { "card" : "Ethernet0" },
         "severity" : "major"
       }
     }
   }
~~~

Cbor Encoding:

~~~
A1                                      # map(1)
   78 1D                                # text(29)
      696574662D68747470732D6E6F7469663A6E6F74696669636174696F6E # "ietf-https-notif:notification"
   A2                                   # map(2)
      69                                # text(9)
         6576656E7454696D65             # "eventTime"
      74                                # text(20)
         323031332D31322D32315430303A30313A30305A # "2013-12-21T00:01:00Z"
      71                                # text(17)
         6578616D706C652D6D6F643A6576656E74 # "example-mod:event"
      A3                                # map(3)
         68                             # text(8)
            7365766572697479            # "severity"
         65                             # text(5)
            6D616A6F72                  # "major"
         6B                             # text(11)
            6576656E742D636C617373      # "event-class"
         65                             # text(5)
            6661756C74                  # "fault"
         70                             # text(16)
            7265706F7274696E672D656E74697479 # "reporting-entity"
         A1                             # map(1)
            64                          # text(4)
               63617264                 # "card"
            69                          # text(9)
               45746865726E657430       # "Ethernet0"
~~~

### CBOR encoding using SIDs as keys

Diagnostic Notation:

~~~ http-response
   {
    2600: {
       1: "2013-12-21T00:01:00Z",
       "example-mod:event" : {
         "event-class" : "fault",
         "reporting-entity" : { "card" : "Ethernet0" },
         "severity" : "major"
       }
     }
   }
~~~

The above is assuming the YANG module for event notifications has a corresponding .sid file with these entries

~~~
"item": [
      {
        "namespace": "module",
        "identifier": "ietf-notification",
        "sid": "2600"
      },
      {
        "namespace": "data",
        "identifier": "/ietf-notification:notification",
        "sid": "2601"
      },
      {
        "namespace": "data",
        "identifier": "/ietf-notification:notification/eventTime",
        "sid": "2602"
      }
    ]
~~~

CBOR Encoding:

~~~
A1                                      # map(1)
   19 0A28                              # unsigned(2600)
   A2                                   # map(2)
      01                                # unsigned(1)
      74                                # text(20)
         323031332D31322D32315430303A30313A30305A # "2013-12-21T00:01:00Z"
      71                                # text(17)
         6578616D706C652D6D6F643A6576656E74 # "example-mod:event"
      A3                                # map(3)
         68                             # text(8)
            7365766572697479            # "severity"
         65                             # text(5)
            6D616A6F72                  # "major"
         6B                             # text(11)
            6576656E742D636C617373      # "event-class"
         65                             # text(5)
            6661756C74                  # "fault"
         70                             # text(16)
            7265706F7274696E672D656E74697479 # "reporting-entity"
         A1                             # map(1)
            64                          # text(4)
               63617264                 # "card"
            69                          # text(9)
               45746865726E657430       # "Ethernet0"
~~~

## Relay Notification Response

The response on success is  "204 (No Content)". In case of corrupted or malformed event, the response is an appropriate HTTP error response.


# Scope of Experimentation

CBOR encoding may be tested against JSON and XML to evaluate requests per second, data transfer rate, and overall network efficiency.

Bandwidth constraints can be applied using traffic control to analyze CBOR encoding efficiency under different network conditions.


# Security Considerations

Addition of the CBOR encoding introduces no specific security exposures or risks other that the ones mentioned in {{!RFC9254}} and {{!I-D.draft-ietf-netconf-https-notif-15}} (An HTTPS-based Transport for YANG Notifications)

# IANA Considerations

This document requests the the IANA registry to include an additional entry to the proposed initial assignments in the “Capabilities for HTTPS Notification Receivers” registry within the YANG Notifications registry group(defined in {{RFC3553}}) as requested in the draft {{!I-D.ietf-netconf-http-client-server}}. The following entry is added :

~~~
Record:
   URN:         urn:ietf:params:yang-notif:https-capability:encoding:cbor
   Reference:   RFC XXXX:An HTTPS-based Transport for YANG Notifications
   Description: Identifies support for CBOR-encoded notifications.
~~~

--- back

# Acknowledgments
{:numbered="false"}

The authors acknowlegde the support of Kent Watsen and Mahesh Jethanandani, the authors of {{!I-D.draft-ietf-netconf-https-notif-15}} for their guidance and support provided to draft this document.
