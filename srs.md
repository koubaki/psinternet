# SRS

## Introduction

SRS (Service Response System) is a system designed to map PSI (Public System of Internetworking) packets to responses sent back to the source from the destination.

It aims to replace ICMP (Internet Control Message Protocol) usage in PSI.

## Terminology

- **SRS Code**: A 1-byte numeric code indicating the type of response.

## Signatures

All valid SRS responses must include an array that contains at least the following data from the node that the currently asked node asked to forward the packet to the service or the node where the SRS code originated from:

- A signature of the rest of the data.

- The identifier of the previous node in the chain (or the node that is currently asked if it's the first element).

- The identifier of the next node in the chain (unless it's the last element).

- The SHA-256 hash of the entirety of the unsigned PSI header (including extension headers but not the payload or signature itself).

- The code if the last element in the array.

## Codes

These are the following codes:

- 0: Packet received.

- 1