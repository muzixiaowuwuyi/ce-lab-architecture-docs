## 1. Security Strategy Overview

The architecture implements a layered security approach (Defense in Depth):
*   **Public Access Control**: Only the Application Load Balancer (ALB) is directly accessible from the internet.
*   **Network Isolation**: The Application Tier is hosted in a Private Subnet with no direct internet route.
*   **Identity-Based Access**: Administrative access is restricted via a Bastion Host (Jumpbox) and limited to a specific management IP.

---

## 2. Security Group Matrix (Inbound Rules)

This matrix defines the allowed inbound traffic for each component. All outbound traffic (Egress) is allowed by default for updates and API calls to AWS services, unless otherwise specified.

| Security Group | Port | Protocol | Source | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **SG-ALB** | 80 | TCP | `0.0.0.0/0` | Allows public HTTP traffic from the internet. |
| **SG-Web** | 80 | TCP | `SG-ALB` | Only accepts traffic forwarded from the Load Balancer. |
| **SG-Web** | 22 | TCP | `SG-Bastion` | Allows SSH management only from the Bastion Host. |
| **SG-App** | 8080 | TCP | `SG-Web` | Accepts API requests/logic calls only from the Web Tier. |
| **SG-App** | 22 | TCP | `SG-Bastion` | Allows SSH management only from the Bastion Host. |
| **SG-Bastion** | 22 | TCP | `Admin-Home-IP/32`| Restricts SSH access to the Administrator's specific public IP. |

---

## 3. Component Justification

### Application Load Balancer (ALB)
The ALB acts as the first line of defense. It terminates external connections and ensures that only valid HTTP requests reach the Web Tier. It hides the internal IP addresses of our EC2 instances.

### Web Tier (Public Subnet)
Though located in the Public Subnet, these instances are protected by `SG-Web`. They do not accept direct traffic from the internet; they only respond to the ALB.

### App Tier (Private Subnet)
The core business logic is completely isolated. It has no Public IP address. This ensures that even if the Web Tier were compromised, the attacker would still need to bypass internal security group rules to reach the Application layer.

### Bastion Host
The Bastion Host is the only gateway for administrative tasks. By limiting the `SG-Bastion` source to a single `/32` IP address, we virtually eliminate the risk of brute-force SSH attacks from the general internet.

---