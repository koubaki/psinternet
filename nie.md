# NIE

## Introduction

NIE (Node Internetworking Ether) is a protocol designed for the transport of PSI (Public System of Internetworking) packets via nodes over any medium.

## Terminology

- **Node**: An entity that participates in the NIE protocol to forward PSI packets.

- **Packet**: A piece of data that is used in the NIE protocol to perform its functions.

## Medium Eligibility

These are the requirements for a medium to be eligible for NIE transport:

- The medium must support the transmission of binary data.

- The medium must fragment NIE packets into smaller units if necessary.

- The medium must include a checksum and length field for each packet.

- The medium must have a direct request-response mechanism for packets.

## Packet Structure

A NIE packet is structured as follows:

## Fields Description

- **Version**: The version of the NIE packet format (unsigned; used for parsing).

- **Authentication**: A big header field used for authentication purposes.

- **Version**: The version of the NIE packet format.

- **Nonce**: A unique value used to prevent replay attacks.

- **Expiration Date**: The date and time after which the packet is considered invalid.

- **Type**: The purpose of the NIE packet.

## Packet Types

- **Data Packet**: Used to carry PSI packets between nodes.

- **Resolution Packet**: Used to resolve LNS (Local Name System) names to PSI public keys.

- **Announcement Packet**: Used to announce the presence of a path to nodes.

## Authentication

The authentication field consists of:

- **Signature**: 73-byte ECDSA signature of the entire packet including the rest of the authentication field.

- **Public Key**: 33-byte compressed PSI public key of the signer.

- **Reserved**: 512 bytes reserved for lower layers to use as needed (e.g., for verifying IP addresses or signing the router's password).

## Packet Payloads

### Data Packet

#### Request

- **Path Length**: The 3-byte length of the path from the source to the node that is sending the packet.

- **Path**: A path from the source to the node that is sending the packet.

- **PSI Packet**: The PSI packet being transported.

#### Response

- **Path Length**: The 3-byte length of the path from the destination back to the source.

- **Path**: A path from the destination back to the source.

### Resolution Packet

#### Request

- **LNS Name**: The LNS name to be resolved.

#### Response

- **PSI Public Key**: The PSI public key corresponding to the resolved LNS name.

### Announcement Packet

#### Request

- **Is Deannouncement**: A boolean indicating whether this is a deannouncement.

- **Path Length**: The 3-byte number of paths being announced.

- **Paths**: The paths being announced to other nodes.

- **Exclusion Length**: The 3-byte number of nodes to exclude from announcing the paths to.

- **Exclusion List**: A list of nodes to exclude from announcing the paths to, including proofs that they have received the paths.

#### Response

- **Confirmation**: A list of proofs confirming receipt of each path.

## Structures

### Data Request Packet Path

The path is an array of the following structure:

- **Signature**: 73-byte ECDSA signature of the rest of the structure.

- **Public Key**: 33-byte compressed public key of the signer.

- **Nonce**: The nonce used in the PSI packet being transported.

- **Next Node**: The next node in the path.

### Data Response Packet Path

