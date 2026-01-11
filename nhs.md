# NHS

## Introduction

NHS (Next Header Selector) is an alternative selector for fields similar to the Next Header field in IP (Internet Protocol).

It assumes that there is no chain of headers, and that the next header is simply the type of the payload (e.g., TCP, UDP, ICMP, etc.).

This is useful in protocols where there is no chaining of headers, such as in protocols where TLVs (Type-Length-Value) replace the traditional header chaining.

## Terminology

- **Next Header**: The type of the payload that follows the current header.

- **NHS Selector**: A two-byte field that indicates the type of the Next Header.

- **Type**: The specific value in the NHS selector that indicates the type of the Next Header.

- **Wrapped Payload**: A payload that is formatted in a specific way indicated by lower layer non-NHS headers, such as encrypted or compressed payloads.

## Format

The NHS selector consists of two bytes.

The first bit indicates whether the payload is wrapped or not.

The second bit is reserved for non-NHS types and is set to 0 for NHS.

The remaining 14 bits are used to indicate the type of the Next Header.

The first 256 types of the reserved non-NHS types are used specifically for the traditional IP Next Header types.

A value of 0 indicates that the payload is not wrapped, while a value of 1 indicates that the payload is wrapped.

## Empty Payload

The first type of the NHS selector (0) is reserved to indicate an empty payload.

## Registration

The NHS selector types other than 0 aren't specified in this document but a registry for NHS selector types may be established.