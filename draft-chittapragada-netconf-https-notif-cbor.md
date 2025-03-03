---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
title: "CBOR Encoding for HTTPS-based Transport for YANG Notifications"
abbrev: "https-notif-ext-cbor"
category: Proposed Standard 

docname: draft-chittapragada-netconf-https-notif-cbor-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: Network Configuration Protocol
workgroup: NETCONF Working Group
keyword:
 - cbor
 - https
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: meherrushi/draft-chittapragada-netconf-https-notif-cbor
  latest: https://example.com/LATEST

author:
 -
    fullname: Bharadwaja Meherrushi Chittapragada
    organization: National Institute of Technology Karnataka, Surathkal
    email: meherrushi2@gmail.com
 -
    fullname: Siddharth Bhat
    organization: National Institute of Technology Karnataka, Surathkal
    email: siddharth.bhat10@gmail.com
 -
    fullname: Vartika T Rao
    organization: National Institute of Technology Karnataka, Surathkal
    email: vartikatrao@gmail.com
 -
    fullname: Hayyan Arshad
    organization: National Institute of Technology Karnataka, Surathkal
    email: hayyanhamnah@gmail.com
 -
    fullname: Mohit P. Tahiliani
    organization: National Institute of Technology Karnataka, Surathkal
    email: tahiliani@nitk.edu.in

normative:
[I-D.ietf-netconf-http-client-server] 
Watsen, K., Jethanandani, M., “An HTTPS-based Transport for YANG Notifications”, Work in Progress, Internet-Draft, draft-ietf-netconf-https-notif-15, 1 February 2024, <https://datatracker.ietf.org/doc/draft-ietf-netconf-https-notif/>

RFC8949:

informative:


--- abstract

This document extends RFCXXXX (I-D.draft-ietf-netconf-https-notif-15) by introducing CBOR encoding for YANG notifications over HTTPS in addition to the existing JSON and XML encoding schemes. 


--- middle

# Introduction

RFCXXXX ({{!I-D.draft-ietf-netconf-https-notif-15}}) introduces a framework for the transfer of YANG notifications over HTTPS using JSON and XML encoding schemes. This document extends RFCXXXX ({{!I-D.draft-ietf-netconf-https-notif-15}}) by introducing CBOR encoding for event notifications over HTTPS in addition to the existing encoding schemes. 

In RFCXXXX, the capabilities HTTP-target resource allows a publisher to retrieve supported encoding formats via a GET request, while the relay-notification resource enables the publisher to send YANG notifications via POST requests. These requests and responses use different content types based on the selected encoding scheme. CBOR offers a more efficient and compact representation of YANG notifications.

Examples of the GET and POST request and reply encoded in CBOR are also provided.  


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Terminology

This document uses the following terms defined in Section 2,3 and 4 of RFCXXXX ({{!I-D.draft-ietf-netconf-https-notif-15}}). 
   1. Capabilities Resource
   2. Relay-Notification
   3. Event Notification

The following term(s) are defined in Subscription to YANG Notifications {{RFC8639}}.
   1. Publisher
   2. Receiver
   3. Subscribed Notifications


# CBOR Encoding

YANG notifications can be encoded in CBOR using Names or SIDs in keys. Notifications encoded using names is similar to JSON encoding as defined in Section 3.4 and 4.3 of RFCXXXX {{I-D.draft-ietf-netconf-https-notif-15}}. Notification encoded using YANG-SIDs replaces the names of the keys of the CBOR encoded message for a 63 bit unsigned integer.  In this case, the keys of the encoded data use the SID value as defined in Section 3.2 of [RFC9254].  

## Request

The publisher sends a request to the receiver to learn its capabilities. In the below example, the “Accept” states that the publisher wants to receive the capabilities response in CBOR but if not supported then in XML or JSON in that order. 

GET /some/path/capabilities HTTP/1.1
   Host: example.com
   Accept: application/cbor, application/xml;0.9, application/json;q=0.5

## Response
	If the receiver is able to reply using “application/cbor” and assuming it is capable of receiving JSON, XML and CBOR encoded messages the response would look like this

### CBOR using names as keys 
Diagnostic Notation: 


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



# Security Considerations

Addition of the CBOR encoding introduces no specific security exposures or risks other that the ones mentioned in RFC 9254 (Encoding of Data Modeled with YANG in the Concise Binary Object Representation (CBOR)) and RFC XXXX [I.D https-notif] An HTTPS-based Transport for YANG Notifications.

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
