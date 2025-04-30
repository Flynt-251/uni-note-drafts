# CN4 - The Transport Layer

We've covered the link layer's role of identifying devices in hops during a transmission, and we've briefly touched up on the transport layer's role, but there are some details that, up to this point, we've glossed over. Now we go into the detail of how the transport layer works, and how data is sent over the internet.

## UDP and chucking packages into windows

Recall the analogy we made between TCP and UDP. While TCP tries to ensure a stable connection, UDP does the digital equivalent of throwing packages at someone's door to deliver them. Will they miss sometimes? Yes, but they don't case.

**User Datagram Protocol** provides the bare minimum services of splitting data into packets, and sending them through to the network layer. If a packet gets lost or corrupted during transit, no effort is made to re-send or recover the data. A device using UDP may also send packets at whatever rate it wishes, regardless of any congestion on the network. |

Of course, the natural question is *"Why?"*, and surprisingly, it's not because of low pay rates. Wait no, not the UPS guy, the UDP. I'm getting myself confused. UDP is useful where losses don't matter, namely streaming, VoIP (voice over internet protocol), discord calls, etc. That's why sometimes one of your friends may say "▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊▊" on a call, because either some packets got corrupted, or went missing.

## TCP and doing things properly

The rest of this page talks about TCP, Transmission Control Protocol. TCP, unlike UDP, takes measures to ensure a reliable transfer of data, flow control and congestion control. || This is more useful for data where losses are significant, such as text data or file transfers. Even under these conditions, packets may still get lost, corrupted or brought out of order, so TCP also has measures to re-send such packets.

- **Checksums** - Numerical value derived from the rest of the data, which is re-calculated on receipt. If there is a mismatch, the packet has corrupted.
- **Acknowledgements** - Informing the sender that packets have been received.
- **Timeouts** - Work with ACKs. If no ACK is received after a certain amount of time, take a new course of action.
- **Retransmission** - Re-send any packets not received, using **Automatic Repeat Request** (ARQ).
- **Sequence Number** - Retain the correct order of packets, regardless of order of arrival.

> Quick note, any time the term "RTT" is used, this is referring to Round Trip Time, i.e. how long it takes for a packet to send successfully, factoring time to send the data, and to receive acknowledgement.

## Stop and Wait ARQ

We'll show this using our old friend Pseudo-C.

```c
/* send_to_host() will give 0 if an ACK is received,
 * otherwise return -1 if timeout is reached.
|/ */

int send(struct* packets) {
    int i = 0;
    int timeout;
    while (i < packets->count) {
        timeout = send_to_host(packets->data[i]);
        if (timeout) {
            i = 0;
        }
    }
}
```

So in essence, if a packet is lost at any time, the sender will start from the beginning of the message. While this is reliable, it's not very efficient. We can calculate utilisation using the following equation:

$U = \frac{L/R}{RTT + L/R} = \frac{L}{R \cdot RTT + L}$

Recall that $L$ and $R$ refer to packet length and network speed respectively. Assuming we have a gigabit connection (10^9 bps), packet size of 10Kb (10,000 bits), and RTT of 20ms, then we get $U = 0.0005$ for sending one bit. Wow, that's very inefficient.

## What are these bits in the pipes?

It of course makes sense to continue sending more data as we wait for the acknowledgement from the server, to improve utilisation. This is a form of **pipelining**. Specifically, we can send, in addition to $L$, $RTT \times R$ bits, otherwise known as the **delay-bandwidth product**. Of course, we need to bear in mind that the server can only handle so much additional information in a buffer of size $B$, before it starts disregarding it due to lack of space. So, the maximum amount of additional data we can send while waiting for acknowledgement is:

$\min (B, L + R \cdot RTT)$

Where $L + R \cdot RTT$ is the length of the pipeline.

