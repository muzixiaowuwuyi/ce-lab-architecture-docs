# System Architecture Documentation

This document describes the architectural design and deployment strategy for the CE-Lab project. The system is designed to follow modern cloud-native best practices for security, scalability, and high availability.

---

## 1. Architectural Pattern: Three-Tier Decoupled Architecture

The system implements a classic **Three-Tier Architecture** (Presentation, Application, and Data), though the Data tier is currently mocked or handled via external APIs for this deployment.

*   **Presentation Tier**: Managed by an **AWS Application Load Balancer (ALB)** and a fleet of **Nginx Web Servers**.
*   **Networking Tier**: Uses a **Virtual Private Cloud (VPC)** with split Public and Private subnets across multiple **Availability Zones (AZs)**.
*   **Application Tier**: Consists of **Node.js instances** handling business logic.


---

## 2. Traffic Flow Analysis

### External Traffic (Ingress)
1.  **User** sends an HTTP request to the ALB’s DNS name.
2.  **ALB** terminates the request and forwards it to an available **Web Server** in the Public Subnet.
3.  **Web Server (Nginx)** processes the request. If it is a dynamic API call, it forwards the request to the **App Server** in the Private Subnet.
4.  **App Server** processes the logic and returns the data back up the chain.