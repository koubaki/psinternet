# PSI

## Introduction

PSI (Public System of Internetworking) is a protocol designed to let nodes transport packets to and from services.

It is meant as a successor of IP (Internet Protocol).

## Terminology

- **Node**: An entity that can process and forward PSI packets.

- **Service**: An application that can be accessed over the PSI network.

- **Packet**: A formatted unit of data carried by the PSI protocol.

- **Address**: A unique identifier assigned to nodes and services within the PSI network.

## Packet Structure

A PSI packet is structured as follows:

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Unsigned Version (1 byte)    |       Hop Limit (2 bytes)      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|       Unsigned Extension Header Length (2 bytes)              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                         Signature (73 bytes)                  |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                     Signature (continued)                     |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                     Signature (continued)                     |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                     Signature (continued)                     |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                     Signature (continued)                     |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Sig (last byte) |                Padding (3 bytes)           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Version (1B)  |                 Nonce (16 bytes)              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Nonce (continued)                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Nonce (continued)                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Nonce (continued)                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Expiration Date (8 bytes)                 |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Expiration Date (continued)               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Extension Header Length (2 bytes)                   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Payload Length (4 bytes)                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|        Next Header (2 bytes)  |  Maximum Hop Limit (2 bytes)  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                     Source Address (33 bytes)                 |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                     Source Address (continued)                |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Source Addr (last byte) |           Padding (3 bytes)         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                  Destination Address (33 bytes)               |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                  Destination Address (continued)              |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Dest Addr (last byte)  |           Padding (3 bytes)          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

Note: There is no padding as shown in the diagram above; it is only meant to match the RFC diagram style.

### Fields Description

- **Unsigned Version**: The version of the PSI protocol (unsigned; used for parsing).

- **Hop Limit**: The maximum number of hops a packet can take before being discarded. Decremented by 1 at each hop.

- **Unsigned Extension Header Length**: The length of the unsigned extension headers section.

- **Signature**: A cryptographic signature to ensure packet integrity and authenticity. Signs the following fields.

- **Version**: The version of the PSI packet format.

- **Nonce**: A unique value used to prevent replay attacks.

- **Expiration Date**: The date and time after which the packet is considered invalid.

- **Extension Header Length**: The length of the extension headers section.

- **Payload Length**: The length of the payload data.

- **Next Header**: Indicates the type of header following the PSI header.

- **Maximum Hop Limit**: The maximum number of hops allowed for the packet. Not decremented.

- **Source Address**: The address of the service sending the packet.

- **Destination Address**: The address of the service intended to receive the packet.

### Fields Format

- **Unsigned Version**: 1 byte. Values: 128-255. Current version is 1 (the byte being 128). First bit is reserved for IP versions and is set to 1 in PSI.

- **Hop Limit**: 2 bytes. Values: 1-65535.

- **Unsigned Extension Header Length**: 2 bytes. Length of the unsigned extension headers in bytes.

- **Signature**: 73 bytes. ECDSA signature using the secp256k1 curve.

- **Version**: 1 byte. Values: 128-255. Current version is 1 (the byte being 128). First bit is reserved for IP versions and is be set to 1 in PSI.

- **Nonce**: 16 bytes. Truly randomly generated UUIDv4.

- **Expiration Date**: 8 bytes. Unix timestamp in milliseconds.

- **Extension Header Length**: 2 bytes. Length of the extension headers in bytes.

- **Payload Length**: 4 bytes. Length of the payload in bytes. Doesn't include either the unsigned or signed extension headers.

- **Next Header**: 2 bytes. Values: 0-65535. Indicates the type of header following the PSI header. Uses the NHS (Next Header Selector) format.

- **Maximum Hop Limit**: 2 bytes. Values: 1-65535.

- **Source Address**: 33 bytes. Compressed ECSDA public key over the secp256k1 curve.

- **Destination Address**: 33 bytes. Compressed ECSDA public key over the secp256k1 curve.

## Extension Headers

In the diagram shown above, there are two extension header length fields.

Immediately after each of these fields, there are extension headers of the specified length.

The first extension header length field specifies the length of unsigned extension headers, while the second specifies the length of signed extension headers.

Unsigned extension headers are not covered by the packet signature, while signed extension headers are.

### Extension Header Format

Both unsigned and signed extension headers follow the same format: Type (32 bytes), Length (2 bytes), Value (variable length).

Type is the identifier of the extension header, Length is the length of the Value field in bytes, and Value is the actual data of the extension header.

Each extension header has a unique Type within the packet, except if it's on different extension header sections (i.e., one in unsigned and one in signed).

When a node doesn't recognize an extension header Type, what happens depends on the Type value:

- If the Type value starts with "S-", the node skips the extension header and continues processing the packet.

- If the Type value starts with "D-", the node discards the packet.

- If the Type value starts with "X-", the nodes does either of the above, at its own discretion.

All valid extension headers have a Type that starts with "S-", "D-", or "X-".

Signed extension headers cannot modified, while unsigned extension headers can be modified by nodes along the packet's route.

The order of extension headers within their respective sections is not significant.

### Common Extension Header Types

#### Hostname

- **Type**: "S-HOSTNAME"

- **Description**: Specifies the hostname of the service.

- **Value**: UTF-8 encoded string representing the hostname.

- **Length**: Variable

- **Signed/Unsigned**: Signed

#### Flow Label

- **Type**: "S-FLOWLABEL"

- **Description**: Specifies a flow label for the packet.

- **Value**: 16-byte. UUIDv4 (truly randomly generated).

- **Length**: 16 bytes.

- **Signed/Unsigned**: Signed

#### Traffic Class

- **Type**: "S-TRAFFICCLASS"

- **Description**: Specifies the traffic class for the packet.

- **Value**: 1 byte. First 5 bits represent the DSCP value (0-63), last 3 bits represent the ECN value (0-3).

- **Length**: 1 byte.

- **Signed/Unsigned**: Either or both

#### Timestamp

- **Type**: "S-TIMESTAMP"

- **Description**: Specifies the timestamp when the packet was created.

- **Value**: 8 bytes. Unix timestamp in milliseconds.

- **Length**: 8 bytes.

- **Signed/Unsigned**: Signed

## Packet Size

If a lower layer protocol has a maximum transmission unit (MTU) smaller than the PSI packet size, PSI packets are always fragmented using protocol-specific fragmentation mechanisms.

PSI itself does not define fragmentation.

PSI has a maximum packet size of approximately 4 gigabytes (without including PSI headers and extension headers), due to the Payload Length field being 4 bytes long.

PSI nodes never reject packets based on size, and lower protocol layers are expected to allow more or the same size as PSI's maximum packet size.

## Flow Label

The Flow Label extension header is a truly randomly generated UUIDv4 (16 bytes).

It is used to identify packets belonging to the same flow, allowing nodes to apply specific handling or routing policies.

## Traffic Class

The Traffic Class extension header consists of 1 byte, where the first 5 bits represent the Differentiated Services Code Point (DSCP) value (0-63), and the last 3 bits represent the Explicit Congestion Notification (ECN) value (0-3).

It is used to classify and manage network traffic.

## Security Considerations

Unlike its predecessor IP, PSI includes built-in security features such as packet signing, nonces, and expiration dates to mitigate various attacks, including spoofing and replay attacks.