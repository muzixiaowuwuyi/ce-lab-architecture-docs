# Future Improvements

This document outlines three key strategic enhancements to improve the system's resilience, scalability, and functionality.

---

## 1. High Availability & Auto Scaling
*   **Current State**: Single instance for App Tier (Single Point of Failure).
*   **Improvement**: Wrap the App Tier in an **Auto Scaling Group (ASG)**.
*   **Benefit**: Automatically adjusts the number of instances based on traffic load and ensures the app stays online if one instance fails.

## 2. Managed Database Integration
*   **Current State**: No persistent data layer.
*   **Improvement**: Deploy an **Amazon RDS (Multi-AZ)** instance.
*   **Benefit**: Provides a secure, scalable, and automated database service with built-in backups and failover capabilities.

## 3. Storage & Delivery Optimization
*   **Current State**: Web Tier handles both logic and static assets.
*   **Improvement**: Offload static files to **Amazon S3** and use **CloudFront (CDN)** for global delivery.
*   **Benefit**: Reduces load on EC2 instances and significantly improves page load speeds for users by serving content from edge locations.