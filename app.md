---
layout: image-right
image: https://photo.pku.edu.cn/Uploads/Bdtpw/Picture/2021/11/08/s6188da15d6e5e.jpg
---

Section 5
# Application

- Smart Room
- Heterogeneous Compute Cluster
- Best Practice
- Future Planing

---

# Smart Room
Application

Frontend: https://pku332.tk (Server is currently under maintenance)

<iframe src="https://pku332.tk" class="w-full h-75%">
</iframe>

---

# Smart Room
Application

Also, there's a introduce video [here](https://www.bilibili.com/video/BV1Dv4y1P72j)

The backend server is a fully containerlized Node.JS application

Temperature and humidity data is collected by a ESP8266 microcontroller

A servo motor is used to control the door

<div class="abs-tr h-full flex items-center justify-end">
  <div class="w-30%">
    <img src="/5.jpg" />
    <img src="/6.png" />
  </div>
</div>

---

# Smart Room
Application

An IR microcontroller is connect to the backend server using \
Serial Port (`/dev/ttyUSB0`)

By sending commands to the microcontroller, we can control the \
air conditional

<div class="abs-tr h-full flex items-center justify-end">
  <div class="w-40%">
    <img src="/7.jpg" />
  </div>
</div>

---

# Heterogeneous Compute Cluster
Application

The next part, Heterogeneous Compute Cluster, is consist of several parts:
- Edge Servers
  - VPS on public clouds
- Local devices
  - Tier 1: Router and low power servers, 24x7 running
  - Tier 2: Desktop PC, wakes up on demand
  - Tier 3: Mobile devices & test servers

Tier 1-2 devices are UPS-protected, and enforcing strict security policies

Tier 3 devices are not protected, and can be replaced easily

---

# Heterogeneous Compute Cluster
Application

However, these server are not fully playing different roles

For most of my services/applications, the deploy location is roughly determined by

- Uptime requirement: prefer VPS for high avaliable services
- Space requirement: prefer local devices for large data storage
- Development status: prefer tier 3 devices for unpolished applications
- User: VPS for public services, local devices for private services
- ...

---

# Heterogeneous Compute Cluster
Application

Examples:

- Blog (WordPress): VPS
- File Server (Samba): Tier 1 Server
- HTTP Tunnel (Cloudflared): Tier 1 Router
- Wash Machine Grabber (In Development): Tier 3 Laptop
- PUBG: Tier 2 Desktop
- ...

---

# Heterogeneous Compute Cluster
Application

However, for save ~~money~~energy devices in Tier 2-3 are not always up

For example, my desktop PC is only powered on when I'm using it

Thus, we need a approach to **remotely wake up** these devices

To implement this, we need to configure and use **Wake-on-LAN** (WOL)

<img src="/7.png" class="h-50%">

---

# Heterogeneous Compute Cluster
Application

Also, sometimes we need to sync files across devices

One useful solution is [syncthing](https://syncthing.net)

However, since we already have a mutual reachable network built, we can simply use [rsync](https://rsync.samba.org)

---

# Best Practice
Application

- Prefer containerlized applications
- For applications that not running inside a container, use systemd to manage them
- For personal small projects, foucus on the functionality and API, then build a frontend on top of it
- **Version manager (eg. git) is highly recommended**
- Regularly take **backups / snapshots**

---

# Future Planing
Application

Currently, there are still some problems that need to be solved

- How to write **pure API**, rather than making a REST API server?
- How to **generate UI automatically** from API?
- How to **manage users and permissions** in a unified way, across multiple applications?

> Build simple, compact, clear, modular, and extensible code that can be easily maintained and repurposed by developers other than its creators.
> -- The Unix philosophy

A new architecture and framework forcusing on solving these problems and enhancing rapid development experience is currently under design and development