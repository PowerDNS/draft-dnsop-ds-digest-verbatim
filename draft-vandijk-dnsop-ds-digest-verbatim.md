%%%
title = "The VERBATIM Digest Algorithm for DS records"
abbrev = "ds-digest-verbatim"
docName = "draft-vandijk-dnsop-ds-digest-verbatim-01+"
category = "std"

ipr = "trust200902"
area = "General"
workgroup = "dnsop"
keyword = ["Internet-Draft"]

[seriesInfo]
name = "Internet-Draft"
value = "draft-vandijk-dnsop-ds-digest-verbatim-01+"
stream = "IETF"
status = "standard"

[pi]
toc = "yes"

[[author]]
initials = "P."
surname = "van Dijk"
fullname = "Peter van Dijk"
organization = "PowerDNS"
[author.address]
 email = "peter.van.dijk@powerdns.com"
[author.address.postal]
 city = "Den Haag"
 country = "Netherlands"


%%%

.# Abstract

The VERBATIM DS Digest is defined as a direct copy of the input data without any hashing.

{mainmatter}

# Introduction

The currently defined DS Digest Algorithms take the input data and hash it into a fixed-length form using well defined hashing algorithms (several SHA variants, and one mostly unused GOST algorithm).
That hashing operation makes any data inside the (C)DNSKEY record unreachable until that data is retrieved from the child zone.
Thus, DS records do not actually convey information; they merely verify information that can be retrieved elsewhere.

A DS record set can only answer the question 'this data that I have here, do you recognise it?'.
In that sense, DS records are not information sources - they are boolean oracles.
For several imagined use cases for signed data at the parent, this might not be sufficient.
One such use case is https://datatracker.ietf.org/doc/draft-schwartz-ds-glue/ [FIXME: make this a proper ref].

This document introduces a new Digest Algorithm, proposed name VERBATIM (alternative suggestion: NULL).
The VERBATIM Digest Algorithm takes the input data (DNSKEY owner name | DNSKEY RDATA per section 5.1.4 of [@!RFC4034]) and copies it unmodified into the DS Digest field.

# Document work

This document lives [on GitHub](https://github.com/PowerDNS/draft-dnsop-ds-digest-verbatim); proposed text and editorial changes are very much welcomed there, but any functional changes should always first be discussed on the IETF DNSOP WG mailing list.

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@RFC8174] when, and only when, they appear in all capitals, as shown here.

# Implementation

The subsection titles in this section attempt to follow the terminology from [@RFC8499] in as far as it has suitable terms.
'Implementation' is understood to mean both 'code changes' and 'operational changes' here.

## Authoritative server changes

None, except where related tooling emits DS records to the administrator.

## Validating resolver changes

Validating resolvers are encouraged to implement the VERBATIM Digest Algorithm.

## Stub resolver changes

This specification defines no changes to query processing in stub resolvers.

## Zone validator changes

Zone validators are encouraged to recognise the VERBATIM Digest Algorithm and, where possible, verify it against the child zone's DNSKEY, if it has any for the given algorithm.

## Domain registry changes

Domain registries are encouraged to allow VERBATIM digests at their user's request.
However, a likely outcome is that domain registries will only allow the VERBATIM digest for DNSSEC algorithms whose specifications call for use of the VERBATIM digest.

# Security Considerations

Previously existing DS Digest Algorithms have a fixed size output.
The VERBATIM digest has a variable size output, that may be under the control of a third party, like the owner of a delegated domain.
Such a third party might cause zone files to grow very big with just a few data submissions to a registrar/registry.
DNS query responses containing VERBATIM digests might also be bigger than is desired.

Implementors, specifically domain registries, may want to limit use of VERBATIM to specified use cases, and with limits appropriate to those use cases.

# Implementation Status

[RFC Editor: please remove this section before publication]


# IANA Considerations

This document updates the IANA registry "Delegation Signer (DS) Resource Record (RR) Type Digest Algorithms" at https://www.iana.org/assignments/ds-rr-types/ds-rr-types.xhtml

The following entry is added to the registry:

~~~ ascii-art
+--------------+----------------+
| Value        | TBD            |
| Description  | VERBATIM       |
| Status       | OPTIONAL       |
| Reference    | RFC TBD2       |
+--------------+----------------+
~~~


# Acknowledgements


{backmatter}

# Document history

