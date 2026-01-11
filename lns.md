# LNS

## Introduction

LNS (Local Name System) is a protocol used for resolving node-specific names to public keys.

## Terminology

- **LNS Name**: A human-readable identifier that maps to a public key.

## Format

A valid LNS name has to:

- Be between 1 and 63 characters long.

- Be in an ASCII format (not Unicode).

- Start with a letter (A-Z, a-z).

- End with a letter or digit (A-Z, a-z, 0-9).

- Contain only letters (A-Z, a-z), digits (0-9), or hyphens (-) in between.

- Not contain consecutive hyphens.

LNS names are case-insensitive.

## Resolution

The public key associated with an LNS name is a compressed ECSDA public key over the secp256k1 curve.