# SRS

## Introduction

SRS (Service Response System) is a system designed to map PSI (Public System of Internetworking) packets to responses sent back to the source from the destination.

It aims to replace ICMP (Internet Control Message Protocol) usage in PSI.

## Terminology

- **SRS Code**: A 1-byte numeric code indicating the type of response.

## Successful Response

0 indicates a successful response, meaning that the packet was successfully delivered to the destination and processed without any issues.

## Registration

A registry of SRS codes may be established in the future, but for now, only 0 is defined to indicate a successful response.