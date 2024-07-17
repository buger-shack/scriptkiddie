# 📹 CCTV / IP Cameras

## Protocols used

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

> **RTSD** : Application-Layer protocol used for commanding streaming media servers via pause and play capabilities. **Most IP cameras use the Real-Time Streaming Protocol** to establish and control video and audio streams. The content is delivered using **RTP** (Real-time Transport Protocol). RSTP does not provide configuration. That must be done using the URI and IP address. **Most systems support RTSP as a fallback even if they are using a different protocol such as PSIA or ONVIF**. _RTSP as an umbrella term describes the **entire stack of RTP** : RTCP (Real-Time Control Protocol), RTSPS (RTSPS over SSL/Secure RTSP) and RTSP_.

> **PSIA** : PSIA is a global consortium of physical security manufacturers and stakeholders that provides a standard for enabling interoperability between security devices. It covers a wide range of security system aspects, including video and analytics, access control, intrusion detection, and other security systems. The PSIA standard defines APIs for products, enabling them to be compatible with systems from other manufacturers that also adhere to the PSIA standard.

> **ONVIF**: ONVIF is an open industry forum that aims to **standardize the communication between IP-based physical security products**. It focuses on ensuring interoperability between network video devices and promotes a global open standard for the interface of IP-based physical security products. ONVIF covers a range of operations including video streaming, device discovery, PTZ (pan, tilt, zoom) control, and more.

> **In essence, PSIA and ONVIF provide a framework for how various security devices can communicate and work together, while RTSP is specifically about how to stream video and audio between devices. A security camera could support ONVIF or PSIA for configuration and control, and also use RTSP for streaming video data to a client or recorder.**

## Common vulnerabilities

The vulnerabilities of protocols like PSIA, ONVIF, and RTSP often revolve around how they are implemented in devices and how the network is configured. Here are some common vulnerabilities associated with these protocols:

**Default Credentials:**

* Many devices come with default administrator credentials which are often not changed by the user. This makes it easy for attackers to gain unauthorized access.

**Unencrypted Communications:**

* Some implementations of these protocols do not use encryption, which means that data, including video streams and control commands, can be intercepted and viewed by unauthorized parties.

**Lack of Authentication and Authorization:**

* Some devices may not properly implement authentication and authorization checks, allowing unauthenticated users to access streams or control the device.

**Firmware Vulnerabilities:**

* Devices may have outdated firmware with known vulnerabilities that can be exploited. Manufacturers might be slow to release patches, or users may neglect to update their devices.

**Insufficient Network Segmentation:**

* Devices on the network may not be properly segmented from other parts of the network, increasing the risk of lateral movement by an attacker after compromising a device.

**Denial of Service (DoS):**

* Protocols may be susceptible to DoS attacks where an attacker overwhelms the device with requests, causing it to crash or become unresponsive.

**Injection Flaws:**

* Poorly implemented protocols may be vulnerable to injection attacks, where an attacker sends malicious commands or queries that are executed by the device.

**Replay Attacks:**

* Without proper safeguards like timestamps or tokens, attackers might capture legitimate commands and replay them to gain control of a device.

**ONVIF-Specific Issues:**

* Some ONVIF devices have been found to be susceptible to XML External Entity (XXE) attacks, allowing attackers to interfere with the XML parsing process.

**PSIA-Specific Issues:**

* PSIA lacks widespread adoption compared to ONVIF, which might mean that security is not as rigorously tested or updated by manufacturers.
