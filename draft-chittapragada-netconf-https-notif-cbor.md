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
area: "Network Configuration Protocol"
workgroup: "NETCONF Working Group"
keyword:
 - cbor
 - https
 - yang

venue:
  group: "NETCONF"
  type: "Working Group"
  mail: "WG@example.com"
  arch: "https://example.com/WG"
  github: "meherrushi/draft-chittapragada-netconf-https-notif-cbor"
  latest: "https://meherrushi.github.io/draft-chittapragada-netconf-https-notif-cbor/draft-chittapragada-netconf-https-notif-cbor.html"

author:
 -
    ins: B. M. Chittapragada
    name: Bharadwaja Meherrushi Chittapragada
    org: National Institute of Technology Karnataka, Surathkal
    email: meherrushi2@gmail.com
 -
    ins: S. Bhat
    name: Siddharth Bhat
    org: National Institute of Technology Karnataka, Surathkal
    email: siddharth.bhat10@gmail.com
 -
    ins: V. T. Rao
    name: Vartika T Rao
    org: National Institute of Technology Karnataka, Surathkal
    email: vartikatrao@gmail.com
 -
    ins: H. Arshad
    name: Hayyan Arshad
    org: National Institute of Technology Karnataka, Surathkal
    email: hayyanhamnah@gmail.com
 -
    ins: M. P. Tahiliani
    name: Mohit P. Tahiliani
    org: National Institute of Technology Karnataka, Surathkal
    email: tahiliani@nitk.edu.in

normative:
 I-D.ietf-netconf-http-client-server-15:
 RFC8949:
 RFC9254:

informative:


--- abstract

This document extends RFCXXXX (I-D.draft-ietf-netconf-https-notif-15) by introducing CBOR encoding for YANG notifications over HTTPS in addition to the existing JSON and XML encoding schemes. 


--- middle

# Introduction

RFCXXXX {{!I-D.draft-ietf-netconf-https-notif-15}} introduces a framework for the transfer of YANG notifications over HTTPS using JSON and XML encoding schemes. This document extends RFCXXXX {{!I-D.draft-ietf-netconf-https-notif-15}} by introducing CBOR encoding for event notifications over HTTPS in addition to the existing encoding schemes. 

In RFCXXXX, the capabilities HTTP-target resource allows a publisher to retrieve supported encoding formats via a GET request, while the relay-notification resource enables the publisher to send YANG notifications via POST requests. These requests and responses use different content types based on the selected encoding scheme. CBOR offers a more efficient and compact representation of YANG notifications.

Examples of the GET and POST request and reply encoded in CBOR are also provided in this document.  


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Terminology

This document uses the following terms defined in Section 2,3 and 4 of RFCXXXX {{!I-D.draft-ietf-netconf-https-notif-15}}. 
   - Capabilities Resource
   - Relay-Notification
   - Event Notification

The following term(s) are defined in Subscription to YANG Notifications {{!RFC8639}}.
   - Publisher
   - Receiver
   - Subscribed Notifications


# CBOR Encoding

YANG notifications can be encoded in CBOR using Names or SIDs in keys. Notifications encoded using names is similar to JSON encoding as defined in Section 3.4 and 4.3 of RFCXXXX {{!I-D.draft-ietf-netconf-https-notif-15}}. Notification encoded using YANG-SIDs replaces the names of the keys of the CBOR encoded message for a 63 bit unsigned integer.  In this case, the keys of the encoded data use the SID value as defined in Section 3.2 of {{!RFC9254}}.  

## Request

The publisher sends a request to the receiver to learn its capabilities. In the below example, the “Accept” states that the publisher wants to receive the capabilities response in CBOR but if not supported then in XML or JSON in that order. 

GET /some/path/capabilities HTTP/1.1
   Host: example.com
   Accept: application/cbor, application/xml;0.9, application/json;q=0.5

## Response
 If the receiver is able to reply using “application/cbor” and assuming it is capable of receiving JSON, XML and CBOR encoded messages the response would look like this

### CBOR using names as keys 

Diagnostic Notation: 

~~~ http-message
   HTTP/1.1 200 OK
   Date: Tue, 4 March 2025 20:33:30 GMT
   Server: example-server
   Cache-Control: no-cache
   Content-Type: application/json
   ~~~ json
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
~~~

# Security Considerations

Addition of the CBOR encoding introduces no specific security exposures or risks other that the ones mentioned in {{!RFC9254}} and RFC XXXX {{!I-D.draft-ietf-netconf-https-notif-15}} (An HTTPS-based Transport for YANG Notifications)

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

The authors acknowlegde the support of the authors of RFC XXXX {{!I-D.draft-ietf-netconf-https-notif-15}} for the encouragment and guidance provided to draft this document .