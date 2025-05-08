# Network Sockets Project

This project explores network programming using the Sockets API by implementing a client and server application that communicate over a custom protocol using both UDP and TCP.

## Project Overview

The project is divided into two parts:

* **Client Implementation:** Client application that interacts with a server following a specific multi-stage protocol to extract secrets at each stage.
* **Server Implementation:** Server application that implements the same protocol, handles multiple clients concurrently, and verifies incoming packets according to the protocol specification.

The primary goals are to gain hands-on experience with:

* UDP and TCP socket programming
* Network byte order (big-endian)
* Handling different data types over sockets (integers, characters, strings)
* Implementing a custom application-level protocol
* Reliable data transfer over unreliable channels (UDP in Stage b)
* Concurrent server design (threading)

## Protocol Details


## Implementation Details

The project can be implemented in any language, but Python or Java are recommended due to the availability of libraries and ease of use for socket programming and byte manipulation.


### Server 

* Listen on UDP port initially.
* Handle multiple clients concurrently using threading.
* For each client, maintain the state of the protocol stages.
* Randomly generate `num`, `len`, `udp_port`, `secretA`, `tcp_port`, `secretB`, `num2`, `len2`, `secretC`, `c`, and `secretD` for each client session.
* Implement the reliable UDP transfer in Stage b, including the random dropping of ACKs (at least once per client session) to test client retransmission.
* Validate all incoming packets (header fields, payload content, lengths, order) and close the connection if any validation fails or if a timeout occurs (3 seconds inactivity).

## How to Build and Run

The project includes bash scripts for running the client and server, designed for easy execution. Information provided in README in child directories.
