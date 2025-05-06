# Network Sockets Project (Based on CSE 461 - Project 1)

This project explores network programming using the Sockets API by implementing a client and server application that communicate over a custom protocol using both UDP and TCP. It is based on the requirements of a university course project (CSE 461 at the University of Washington).

## Project Overview

The project is divided into two parts:

* **Part 1: Client Implementation:** Develop a client application that interacts with a server following a specific multi-stage protocol to extract secrets at each stage.
* **Part 2: Server Implementation:** Develop a server application that implements the same protocol, handles multiple clients concurrently, and verifies incoming packets according to the protocol specification.

The primary goals are to gain hands-on experience with:

* UDP and TCP socket programming
* Network byte order (big-endian)
* Handling different data types over sockets (integers, characters, strings)
* Implementing a custom application-level protocol
* Reliable data transfer over unreliable channels (UDP in Stage b)
* Concurrent server design (threading)

## Protocol Details

The communication protocol is stateful and proceeds through four stages (a, b, c, d). Both client and server packets include a 12-byte header.

### Packet Header Format

Every packet (UDP and TCP, sent and received) includes this header:
