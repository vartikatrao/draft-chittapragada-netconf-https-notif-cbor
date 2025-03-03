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


informative:


--- abstract

This document extends RFCXXXX (I-D.draft-ietf-netconf-https-notif-15) by introducing CBOR encoding for YANG notifications over HTTPS in addition to the existing JSON and XML encoding schemes. 


--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
