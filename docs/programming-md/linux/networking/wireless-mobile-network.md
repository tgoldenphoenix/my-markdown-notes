# Wireless and Mobile Networks

## Terminologies

`modulating` a Wi-Fi signal means taking the raw digital data (the 1s and 0s from your computer) and "piggybacking" them onto a radio wave so they can travel through the air.

## WiFi: 802.11 Wireless LANs

The `IEEE 802.11 wireless LAN` is the set of standards that defines **Wi-Fi**. It's part of the IEEE 802 family of networking standards. The 802.11 standard specifically describes how wireless communication works at the Data Link (Layer 2) and Physical (Layer 1) layers of the OSI model.

When you see different versions like `802.11g`, 802.11n, 802.11ac, or 802.11ax (Wi-Fi 6), these are just amendments to the original standard that specify new, faster, or more efficient ways to transmit data wirelessly.

Yes, the 802.11 (Wi-Fi) standard operates at both the Data Link Layer (Layer 2) and the Physical Layer (Layer 1) of the OSI model.

## Air as the Medium

Another consequence of using an unbounded medium is that devices must operate in `half-duplex mode`; only one device can transmit at a time, or their signals will collide. Wireless devices must contend for airtime—they compete for the chance to transmit data over a shared medium. This half-duplex nature is a significant factor in why wireless networks often do not match the speeds of wired networks using switches (which operate in full-duplex mode).

### Wave Behaviors

The electromagnetic waves that are used to encode wireless signals are influenced by the media they pass through and objects they encounter.

`Absorption` occurs when a wave passes through a medium and is converted into heat, weakening the signal. For example, a wireless signal passing through a wall can cause significant attenuation, particularly if the wall is made of a dense material. This can prevent devices on the other side of the wall from receiving a coherent signal.

`Attenuation` is the weakening of a signal.

`Reflection` happens when a wave bounces off a surface rather than passing through it; this is the same phenomenon as light reflecting off a mirror. Metal surfaces are common culprits, as they are highly reflective of radio waves. Reflection is the reason wireless reception is usually poor in elevators; the signal bounces off the metal, and very little penetrates into the elevator.

`Refraction` occurs when a wave passes from one medium to another one with a different density, altering the wave’s speed and causing it to bend. A common everyday occurrence of refraction is the apparent shift in the position of an object in water when viewed from above; this is due to the light waves bending as they move from water to air.

`Diffraction` is the bending and spreading of waves around the edges of an obstacle, such as a wall. This enables wireless communication even when there isn’t a clear line of sight between the transmitter and receiver. In urban environments, diffraction often allows signals to reach street level and indoor areas that are not in direct view of the cell tower or wireless access point.

The final phenomenon is `scattering`, which occurs when a wave encounters a surface or medium with irregularities that cause the wave to spread out erratically. Common causes of scattering are dust, smog, water vapor, and textured surfaces. Scattering can cause signal attenuation as it disperses the signal’s energy in various directions.

## Radio frequency

To send a wireless signal, a device applies an alternating electric current to an antenna, which in turn produces fluctuating electromagnetic fields that radiate out as waves; these are called electromagnetic waves. Two key measurements of an electromagnetic wave are its amplitude and frequency.

`Amplitude` (biên độ) measures the maximum strength of the electric and magnetic fields of a wave and is associated with how much energy the wave carries; basically, higher amplitude means a stronger signal. If the amplitude of a signal is too low, the receiver won’t be able to distinguish a coherent message from the signal.

`Frequency` (tần số) measures how quickly the strength of the wave’s electric and magnetic fields oscillates and is measured in hertz (Hz)—the number of oscillations per second. Like bits and bytes, hertz are typically measured in thousands, millions, billions, and trillions (or even greater):

- `kHz` (kilohertz)—1000 cycles per second
- `MHz` (megahertz)—1,000,000 cycles per second = $1000$ kHz
- `GHz` (gigahertz)—$1,000,000,000$ cycles per second
- `THz` (terahertz)—1,000,000,000,000 cycles per second

The amplitude is how high the wave is. The frequency is how "packed" together the wave is.

Frequency & wave length are inversely proportional. This means that as one goes up, the other must go down.

A related concept is `period`, which is the amount of time it takes for one full oscillation.

In the context of radio waves, `frequency` determines a wave’s position within the electromagnetic spectrum and has a major effect on how the signal behaves as it propagates through the air and other media.

In wireless LANs, **higher frequencies can typically support higher data transfer rates** and are less crowded with other wireless devices. However, they are also more susceptible to absorption by obstacles

On the other hand, **lower frequencies penetrate better** through obstacles and can, therefore, cover larger distances. However, lower frequencies typically support lower transfer rates and are more crowded with other wireless devices.

---

The `electromagnetic spectrum` is the entire range of electromagnetic radiation, such as AM and FM radio, ultraviolet light, X-rays, and gamma rays. It goes from low frequency on the left side to high frequency on the right side.

`Radio frequency (RF)` is a segment of the electromagnetic spectrum generally defined as ranging from around **20 kHz to around 300 GHz**.

RF is used for a variety of purposes, from AM and FM radio to microwaves and radar. However, most relevant to this chapter are 802.11 wireless LANs. Three bands within the RF range are used for wireless LANs: the 2.4 GHz band, the 5 GHz band, and the newer 6 GHz band, introduced to wireless LANs in 2020.

A `band` is a specific range of frequencies, such as the AM radio band, FM radio band, and the 802.11 wireless LAN bands.

---

The `2.4 GHz band` spans from **2.400 to 2.495 GHz** and is widely used for wireless communications due to its lower frequency, which enables better penetration through obstacles compared to the 5 GHz and 6 GHz bands.  
However, this band is **also used for other technologies**, such as Bluetooth, microwave ovens, cordless telephones, and many others. This means that it can sometimes be crowded, leading to congestion and interference.

The 2.4 GHz band is divided into $14$ individual `channels`. Like a band, a channel is a specific range of frequencies—you can think of a channel as **a smaller division of a band**. For wireless communication to occur, both the transmitting and receiving devices must be **tuned to the same channel**, enabling them to “speak” and “listen” on the same frequency range.

Each channel has a defined center frequency and a standard width of 20 MHz, although older standards use 22 MHz. Wireless devices communicate using these channels, so careful channel selection is critical to minimize interference. 

Not all channels in the 2.4 GHz band can be used in all countries. For example, only channels 1 to 11 are commonly allowed in the United States/Canada, while most countries allow 1 to 13. Channel 14 is allowed in Japan, but only when using an older 802.11 standard.

There is significant overlap between the 2.4 GHz channels. In a wireless LAN that needs multiple APs for full coverage, it’s important to use non-overlapping channels to avoid interference. If adjacent access points use the same channel, devices not only have to contend for airtime with other devices using the same access point but with devices using neighboring access points too. **Channels 1, 6, and 11** are recommended

---

The 5 GHz band, ranging from approximately **5.150 to 5.895 GHz** (depending on the country), is the second frequency band widely used for wireless LANs. Compared to the 2.4 GHz band, the 5 GHz is generally **less crowded** and not as prone to interference from common household devices. 

## Service Sets

In the 802.11 standards, a service set is a group of devices that operate on the same wireless LAN, sharing the same `service set identifier (SSID)`—a human-readable label that identifies the service set.

When you use 802.11 (Wi-Fi) to connect to a wireless network from your smartphone or laptop, the network name you select is that network’s SSID.

---

A `basic service set (BSS)` forms the fundamental building block of an 802.11 wireless LAN. In a BSS, wireless clients connect to a wireless access point (abbreviated as WAP or AP), which coordinates the communication between the devices and serves as the gateway to other network resources, such as a wired LAN or the internet.

The area around an AP where clients can successfully communicate with it—the AP’s coverage area—is called a `basic service area (BSA)` or `cell`.

The SSID of the BSS, `Jeremy’s Wi-Fi`, is a human-readable name that serves as the network’s identifier for users. **SSIDs do not need to be unique**. Instead, a basic service set identifier (BSSID) serves to uniquely identify the BSS. While multiple BSSs may use the same SSID to create an extended service set (ESS) for wider coverage, each BSS will have a unique BSSID

The BSSID is the MAC address of the AP’s radio. MAC addresses are uniquely assigned to the device by the manufacturer; no two devices will have the same MAC.

---

Most wireless LANs aren’t standalone networks. Rather, wireless LANs are a way for wireless clients to connect to the wired network infrastructure, enabling wireless clients to communicate with hosts in other BSSs, hosts in the wired LAN, in remote sites via the WAN, over the internet, etc. 802.11 calls the wired network infrastructure the `distribution system (DS)`. Although the previous diagrams omitted the DS, a wireless LAN without a DS is rare. Without a DS, an AP’s wireless clients can only communicate among themselves; they have no gateway to other networks.

In addition to its wireless radio, each AP has an Ethernet port through which it can connect to the DS (usually via a switch). The AP serves as a bridge connecting the two mediums: it translates 802.11 frames from wireless clients to Ethernet frames to be sent over the wired LAN, and vice versa.

## Base Station

**Cell towers** in cellular networks and access points in 802.11 wireless LANs are examples of base stations.

They don't act like routers. They act more like bridges or "wireless switch ports" that connect you to a router. An access point or cell tower's main job is at Layer 2 (Data Link Layer): to connect your device (a "host") to its local network.

the base station is connected to the larger network (e.g., the Internet,  corporate or home network), thus functioning as a link-layer relay between the wireless host and the rest of the world with which the host communicates.