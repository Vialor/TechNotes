**Framing** 封装成帧: In layer 2, layer 3 packets are wrapped into frames which usually contains: head, body(packet) and tail
Data link layer in all the 802 protocols is split into two or more sublayers:
**MAC (Medium Access Control) sublayer** controls access to the shared channel.
**LLC (Logical Link Control) sublayer** hides the differences between the different 802 variants and make them indistinguishable as far as the network layer is concerned. This could have been a significant responsibility, but these days the LLC is a glue layer that identifies the higher-layer protocol (e.g., IP) to which the payloads should be passed
[[Frames]]
[[Wired LAN]]
[[Wireless and Mobile Networks]]