## MAC Address
> aka **Physical Address**, **Ethernet Address**

MAC addresses are used to identify hosts in a common broadcast channel; all frames contain MAC addresses of senders and receivers.
MAC addresses go with the EEPROM chips in the network adaptors, so one machine can have many MAC addresses.
Each MAC address is unique in the world.
**EUI-48**: MAC addresses have 6 bytes and can be displayed as: `00:0C:CF:93:8C:92` or `00-0C-CF-93-8C-92`; first 3 bytes is **OUI (Organizational Unique Identifier)** showing the manufacturer of the device, the last 3 bytes is the **extended identifier**. `FF-FF-FF-FF-FF-FF` is a broadcast address; multicast addresses start with bit 1.
## Frame Structure
Preamble
Start Frame Delimiter, SFD
Dest MAC
Src MAC
EtherType: network protocols or data length
...
Data
Frame Check Sequence, FCS