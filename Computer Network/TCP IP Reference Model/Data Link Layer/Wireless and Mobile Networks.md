**BS Base Station**: a device that allows wireless hosts to connect to a wired computer network  
In cellular networks,  
**Cell Tower** is the base station. In 802.11 wireless LAN, **AP Access Point** is the base station.
**BSS Basic Service Set**: a set of wireless devices which can communicate with each other
	**IBSS Independent BSS / Ad-hoc Network**: A network where devices communicate with each other without an AP
	**Infrastructure BSS**: A network topology that allows all wireless devices to communicate with each other through exactly one base station.
**Handoff/Handover**: the process of wireless hosts switching their point of attachment to another base station
Access Points turn the 802.11 frames from the wireless host into 802.3 frames, which are sent to the attached router.  
802.11 frames contain MAC of AP, MAC of the wireless host, MAC of the router interface AP is attached to  
802.3 frames contain MAC of the router (dest address), MAC of the wireless host(src address)  
## Challenges of WLAN
1. decreasing signal strength through transmission mediums; interference from radio sources of the same frequency band
2. multipath propagation: electromagnetic waves travels paths of different lengths to the receiver causing the blurring of signal.
3. half-duplex nature: cannot send and receive at the same time
## WLAN Architecture
**Wi-Fi Jungle**: physical location where multiple APs are accessible
**Beacon Frames**: As required in 802.11, APs periodically send beacon frames informing wireless hosts of their presence. Beacon frames contains MAC and SSID (**Service Set Identifier**, the names in the Wi-Fi list) of the AP.
- Two ways for a wireless host to find AP:
    **Passive Scanning**:
    1. AP to host: beacon frame
    2. host to AP: Association Request frame
    3. AP to host: Association Response frame
    **Active Scanning**:
    1. host broadcasts: Probe Request frame
    2. AP to host: Probe Response frame
    3. host to AP: Association Request frame
    4. AP to host: Association Response frame
## CSMA **Carrier Sense Multiple Access/Collision Avoidance**

> Used in **IEEE 802.11**, which is an WLAN standard for WIFI  
> CSMA/CA = CSMA + avoid collision before transmitting data  
> Collision detection is difficult for WLAN due to fading noise and the half-duplex nature  
### CSMA/CA Mechanism
1. Carrier Sense: Physical sensing + Virtual sensing
    
    Physical sensing: simply checks the medium to see if there is a valid signal.
    
    Virtual sensing: each station keeps a logical record of when the channel is in use by tracking the  
      
    **NAV Network Allocation Vector**
    
    **NAV**: Each frame carries a _duration_ field that specifies the transmission time required for the frame. Stations that overhear this frame set their NAV based on _duration_, which is an indicator for a station on how long it must defer from accessing the medium
    
2. Collisions Avoidance:  
    1. senders starts with a random backoff  
    2. use ACK after data to infer collisions because collisions cannot be detected (When ACK is not received, sender considers it an collision)  
    
### Problems of CSMA/CA
**Hidden terminals**: senders that do not see each other but collide at intended receivers.
**Exposed terminals**: senders that see each other but still transmit safely (to different receivers)
### RTS/CTS Collision Avoidance

> Optionally used as an supplement to CA to solve Hidden terminals and Exposed terminals problem.  
> RTSs can still collides, but they are short. The idea is to avoid collisions of long data frames.  
1. A _broadcasts_ RTS (request to send) frame to B (containing _duration_)
2. B _broadcasts_ CTS (clear to send) frame to A (containing _duration_)
3. A reserves the transmission channel by NAV and sends data frames to B
4. B _broadcasts_ ACK upon receipt
(If A does not receive CTS, there is a collision. A backs off based on Exponential Backoff Algorithm.)
Any node that sees CTS knows it cannot transmit (Hidden terminals problem solved)  
  
**MACA Multiple Access with Collision Avoidance**: Any node that sees RTS but not CTS is free to transmit. (Exposed terminals problem solved, but does not have ACK)  
CSMA/CA: Any node that sees RTS or CTS cannot transmit. (Exposed terminals problem not solved, CSMA/CA behaves this way to make sure sender can receive ACK)  
As such, MACA is an early unreliable solution, yet CSMA/CA is reliable.  
## Advanced features in 802.11
**Rate Adaptation**: algorithms to dynamically secelct the proper modulation scheme
SNR Signal-to-Noise Ratio  
BER Bit Error Rate  
Modulation Scheme/Technique: different schemes have different bit transmission rate (Mbps)  
Given modulation scheme, SNR & BER are negatively related, but increasing signal strength also means more energy waste and is unfair to other senders  
Given SNR, BER & modulation scheme bit transmission rate are positively related  
**Power Management**: algorithms to allow clients to sleep and save power
Clients can set a power-management bit in frames that they send to the AP to tell it that they are entering power-save mode. In this mode, the client can doze and the AP will buffer traffic intended for it. To check for incoming traffic, the client wakes up for every beacon, and checks a traffic map that is sent as part of the beacon. This map tells the client if there is buffered traffic. If so, the client sends a poll message to the AP, which then sends the buffered traffic. The client can then go back to sleep until the next beacon is sent