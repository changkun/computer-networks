# Motivation

We have been so far:

- How digital data is represented? Transmitted and reconstructed by Measureable Quantifies (Layer 1)
- How the access to the transmission medium is controlled and the respective next-hop is addressed (Layer 2)
- How hosts are addressed end-to-end based on logical addresses and that data is packed oriented (Layer 3)

The essential tasks of the transport layer are:

- Multplexing data streams from different applications or application instances
- Providing connectionless and connection-oriented transport mechanisms
- Mechanisms for congestion and flow control

Multiplexing:

- Segmentation of the data streams of different applications (browser, chat, email, ...)
- Segments are routed to the receiver in independent IP packets
- The receiver must assign the segments to the individual data streams and forward them to the respective application.

Transport services:

- Connectionless (Best Effort)
    - Segments are from the perspective of the transport layer independent from each other
    - No sequence numbers, no trasmission repetition, no guarantee of the correct order
- Connection-oriented
    - Transmission repeat on erros
    - Guarantee the correct order of individual segments

Congestion and flow control:

- Congestion control
    - Reaction to imminent overload in the network
- Flow control
    - Load control by the receiver