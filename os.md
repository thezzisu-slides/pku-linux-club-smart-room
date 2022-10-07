---
layout: image-right
image: https://photo.pku.edu.cn/Uploads/Bdtpw/Picture/2021/11/08/s6188db2927682.jpg
---

# Operating System

- Simple Comparison of Linux distributions
- Best Practice

---

# Simple Comparison of Linux distributions
Operating System

Now we have a network of connected devices, we need to choose the right operating system for each of them.

For servers, Linux is the best choice. And for choosing a suitable Linux distro, we need to consider the following factors:

- Packages: How many packages are available in the official repository?
- Community: Wiki/Forum/Issue Tracker
- Stability: Will buggy updates break my system?
- Sustainability: Can I upgrade to the latest supported release without breaking my system?

---

# Simple Comparison of Linux distributions
Operating System

| Distro         | Arch       | Fedora | Ubuntu | CentOS      | Alpine | OpenWRT |
|----------------|------------|--------|--------|-------------|--------|---------|
| Packages       | Everything | Lot of | Lot of | Sufficient  | Basic  |   -     |
| Community      | Very Good  | Good   | Good   | Good        | New    | Good    |
| Stability      | Unstable   | Stable | Stable | Very Stable | Stable | Stable  |
| Sustainability | Good(rolling) | Good(officially supported) | Good(officially supported) | Poor(not supported) | Unknown | Poor(reflash required for EXT4) |
| Security       | SELinux(optional) | SELinux(default) | AppArmor | SELinux(default) | - | - |
| Status         | Active | Active | Active | Dead | Active | Active |

---

# Simple Comparison of Linux distributions
Operating System

My choice:
- Desktop: Arch
- Server: Fedora (Server Edition)
- Router: OpenWRT

---

# Best Practice

- Check SELinux for unexpected permission errors
  - Do not turn off SELinux. Instead, set the correct flag
- Disable SSH password login, use only privkey authorization
- Constantly take **snapshots**
  - Use LVM / Btrfs