# Network Sockets Project

This project explores network programming using the Sockets API by implementing a client and server application that communicate over a custom protocol using both UDP and TCP.

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

```
0               1               2               3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                          payload_len                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                            psecret                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|              step             |   last 3 digits of student #  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```


*The diagram omits the header.*

### STAGE b (UDP with Reliability)

* **Step b1 (Client -> Server, with ACKs):** The client reliably transmits `num` UDP packets to the server on port `udp_port`. Each of these ‘data’ packets has a payload length of `len + 4` bytes (remembering 4-byte alignment for the total packet). The first 4-bytes of each data packet payload must be a uint32_t identifying the packet (`packet_id`), from 0 to `num - 1`. The rest of the payload bytes (`len` of them) must be 0s. The header's `psecret` is `secretA`, and `step` is 1.

0               1               2               3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                           packet_id                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                     payload of length len                     |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+


*The diagram omits the header.*

For each received data packet, the server will acknowledge (ack) that packet by replying with an ‘ack’ packet that contains as the payload the uint32_t identifier of the acknowledged packet:

0               1               2               3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        acked_packet_id                        |
+-+-+-+-+-+-+-+-+-+-+-+-

You stopped this response





give me one big markdown file



Show thinking
Okay, here is the content formatted as a single standard Markdown (.md) file.

Markdown

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

0               1               2               3
0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                          payload_len                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                            psecret                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|              step             |   last 3 digits of student #  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+


* `payload_len` (uint32\_t): The length of the payload data *excluding* the header and any padding.
* `psecret` (uint32\_t): The secret obtained from the *previous* stage. For Stage a, this is 0.
* `step` (uint16\_t): The current step number within the stage (e.g., 1 for client steps, 2 for server responses).
* Last 3 digits of student \#: (uint16\_t): A unique identifier (using a placeholder value for a personal project).

All multi-byte data (integers, `payload_len`, `psecret`, `step`, student ID digits) must be in network byte order (big-endian). Packets must be padded with zeros to ensure the total packet length (header + payload + padding) is a multiple of 4 bytes.

### STAGE a (UDP)

* **Step a1 (Client -> Server):** The client sends a single UDP packet containing the UTF-8 string “hello world” without the quotation marks and encoded as utf-8, to the server on port 12235. The header’s `psecret` is 0, and `step` is 1.

    ```
     0               1               2               3
     0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                                                               |
    |     h         |      e        |        l      |       l       |
    |                                                               |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |               |               |               |
    |     o         |               |       w       |  ...
    |               |               |               |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    ```

    *Note: Don’t forget the string terminator (`\0`)! The diagram omits the header.*

* **Step a2 (Server -> Client):** The server validates the packet. If valid, it responds with a UDP packet containing four uint32\_t integers: `num`, `len`, `udp_port`, `secretA`. The header's `psecret` is 0, and `step` is 2.

    ```
     0               1               2               3
     0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                              num                              |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                              len                              |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                            udp_port                           |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                            secretA                            |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    ```

    *The diagram omits the header.*

### STAGE b (UDP with Reliability)

* **Step b1 (Client -> Server, with ACKs):** The client reliably transmits `num` UDP packets to the server on port `udp_port`. Each of these ‘data’ packets has a payload length of `len + 4` bytes (remembering 4-byte alignment for the total packet). The first 4-bytes of each data packet payload must be a uint32\_t identifying the packet (`packet_id`), from 0 to `num - 1`. The rest of the payload bytes (`len` of them) must be 0s. The header's `psecret` is `secretA`, and `step` is 1.

    ```
     0               1               2               3
     0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                           packet_id                           |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                                                               |
    |                     payload of length len                     |
    |                                                               |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    ```

    *The diagram omits the header.*

    For each received data packet, the server will acknowledge (ack) that packet by replying with an ‘ack’ packet that contains as the payload the uint32\_t identifier of the acknowledged packet:

    ```
     0               1               2               3
     0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                        acked_packet_id                        |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    ```

    *The diagram omits the header.*

    To complete this step, the client must receive ack packets from the server for all `num` packets that it generates. The client resends those packets that the server does not acknowledge. The client should use a retransmission interval of at least 0.5 seconds. The server (Part 2) should randomly decide *not* to send an ack for at least one packet during this stage for each client transaction to test the client's reliability implementation. This step completes after the server receives all `num` packets correctly in order.

* **Step b2 (Server -> Client):** Once the server receives all `num` packets, it sends a UDP packet containing two uint32\_t integers: `tcp_port` and `secretB`. The header's `psecret` is `secretA`, and `step` is 2.

    ```
     0               1               2               3
     0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                            tcp_port                           |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                            secretB                            |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    ```

    *The diagram omits the header.*

### STAGE c (TCP)

* **Step c1 (Client -> Server):** The client establishes a TCP connection to the server on the `tcp_port` received in b2.
* **Step c2 (Server -> Client):** The server accepts the TCP connection and sends a TCP packet containing three uint32\_t integers (`num2`, `len2`, `secretC`) and one 1-byte character (`c`). The header's `psecret` is `secretB`, and `step` is 2.

    ```
     0               1               2               3
     0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                              num2                             |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                              len2                             |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                            secretC                            |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |       c       |             (Padding to 4-byte align)         |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    ```

    *The diagram omits the header.*

### STAGE d (TCP)

* **Step d1 (Client -> Server):** The client sends `num2` TCP payloads, each payload of length `len2`, and each payload containing all bytes set to the character `c` received in c2. Each payload should be sent in a separate TCP packet with the standard header. The header's `psecret` is `secretC`, and `step` is 1.

    ```
     0               1               2               3
     0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                                                               |
    |           payload of length len2 filled with char c           |
    |                                                               |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    ```

    *The diagram omits the header.*

* **Step d2 (Server -> Client):** The server receives and validates the `num2` payloads. If valid, it responds with a single TCP packet containing one uint32\_t integer: `secretD`. The header's `psecret` is `secretC`, and `step` is 2.

    ```
     0               1               2               3
     0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                            secretD                            |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    ```

    *The diagram omits the header.*

## Implementation Details

The project can be implemented in any language, but Python or Java are recommended due to the availability of libraries and ease of use for socket programming and byte manipulation.

### Data Representation

* Integers (uint32\_t, uint16\_t) must be sent and received in network byte order (big-endian).
* Characters are 1 byte.
* Strings are UTF-8 encoded and null-terminated (`\0`).

### Server Requirements (Part 2)

* Listen on UDP port initially.
* Handle multiple clients concurrently using threading.
* For each client, maintain the state of the protocol stages.
* Randomly generate `num`, `len`, `udp_port`, `secretA`, `tcp_port`, `secretB`, `num2`, `len2`, `secretC`, `c`, and `secretD` for each client session.
* Implement the reliable UDP transfer in Stage b, including the random dropping of ACKs (at least once per client session) to test client retransmission.
* Validate all incoming packets (header fields, payload content, lengths, order) and close the connection if any validation fails or if a timeout occurs (3 seconds inactivity).

## How to Build and Run

The project includes bash scripts for running the client and server, designed for easy execution. Information provided in README in child directories.
