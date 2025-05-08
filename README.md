# Sorteo Prize Draw Verification Repository

## Overview

This repository contains cryptographic verification data for prize draws conducted on the Sorteo platform. It serves as an immutable public record of the verification process, ensuring complete transparency and fairness in our prize draws.

## Purpose

The purpose of this repository is to provide:

1. A timestamped record of verification data at each stage of a prize draw
2. Proof that prize draw commitments were made before winners were selected
3. A public, immutable audit trail of all verification steps
4. Open access to verification data for independent validation

## Repository Structure

Each prize draw has its own directory with the following structure:

```
/draws/{prizeDrawId}/
  |-- commitment.json       # Contains the commitment data
  |-- ticket-list.csv       # Contains the complete ticket list
  |-- reveal.json           # Contains the reveal data
```

### commitment.json

This file contains the initial commitment data created before the prize draw begins:

```json
{
  "prizeDrawId": "uuid-of-prize-draw",
  "prizeDrawName": "Name of the prize draw",
  "commitmentHash": "sha256-hash-of-local-seed-and-block-number",
  "localSeed": "secure-random-string-generated-locally",
  "blockNumber": 12345678,
  "ticketListHash": "sha256-hash-of-ticket-list-csv",
  "ticketCount": 123,
  "createdAt": "2023-05-01T12:00:00Z"
}
```

### ticket-list.csv

This file contains the complete list of tickets in CSV format. For privacy reasons, email addresses are redacted.

### reveal.json

This file contains the reveal data after the target Ethereum block is mined:

```json
{
  "prizeDrawId": "uuid-of-prize-draw",
  "prizeDrawName": "Name of the prize draw",
  "commitmentHash": "sha256-hash-of-local-seed-and-block-number",
  "localSeed": "secure-random-string-generated-locally",
  "blockNumber": 12345678,
  "ticketListHash": "sha256-hash-of-ticket-list-csv",
  "ticketCount": 123,
  "blockHash": "ethereum-block-hash",
  "combinedSeed": "sha256-hash-of-local-seed-and-block-hash",
  "winningTicketNumber": 42,
  "winnerName": "Winner Name",
  "revealedAt": "2023-05-01T14:00:00Z"
}
```

## Verification Process

The verification process works as follows:

1. **Commitment Creation**: When a prize draw is created, we generate a secure random local seed and target a future Ethereum block. We create a commitment hash using these values.

2. **Ticket List Hashing**: We create a SHA-256 hash of the complete ticket list CSV.

3. **Commit to GitHub**: We immediately commit this data to GitHub, creating a timestamped record that proves the commitment was made before the draw.

4. **Randomness Reveal**: After the target Ethereum block is mined, we retrieve its hash and combine it with our local seed to generate the final randomness.

5. **Winner Selection**: Using the combined seed, we deterministically select a winning ticket.

6. **Reveal Commit**: We commit the reveal data to GitHub, completing the verification process.

## Independent Verification

Anyone can independently verify a prize draw by:

1. Confirming that the commitment.json file was committed before the reveal.json file
2. Verifying that the commitment hash matches SHA256(localSeed + ":" + blockNumber)
3. Checking that the block hash matches the actual Ethereum block hash
4. Confirming that the combined seed matches SHA256(localSeed + ":" + blockHash)
5. Verifying that the winning ticket was selected deterministically using the combined seed

## About Sorteo

Sorteo is a blockchain-powered charity prize draw platform that revolutionizes fundraising through transparent, secure, and engaging digital experiences. Our platform uses advanced cryptographic techniques to ensure fairness and transparency in all prize draws.