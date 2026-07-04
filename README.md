# Wazuh SIEM Deployment using Docker

> Enterprise-style deployment of the Wazuh Security Information and Event Management (SIEM) platform using Docker in an Ubuntu virtual machine. This project documents the complete deployment process, architecture, verification, troubleshooting, and lessons learned while building a practical Security Operations Center (SOC) home lab.

---

## Project Overview

Security Information and Event Management (SIEM) platforms are an essential part of modern Security Operations Centers (SOCs). They collect, process, correlate, and visualize security events from multiple systems, enabling analysts to detect suspicious activity and investigate incidents efficiently.

This project demonstrates the deployment of a complete Wazuh SIEM environment using Docker Compose inside an Ubuntu virtual machine. The objective was not simply to install Wazuh, but to understand its architecture, verify that every component functioned correctly, troubleshoot deployment issues, and create professional documentation for future reference.

The deployment includes the following core services:

- Wazuh Dashboard
- Wazuh Manager
- Wazuh Indexer
- Docker Engine
- Ubuntu 24.04 LTS

This repository serves as the foundation for future cybersecurity projects involving endpoint monitoring, attack simulation, log analysis, detection engineering, and incident response.

---

## Project Objectives

- Deploy Wazuh using Docker Compose.
- Understand the architecture of a Docker-based SIEM platform.
- Verify successful communication between all Wazuh services.
- Document deployment, verification, and troubleshooting.
- Build a professional cybersecurity portfolio project.

---

## Lab Architecture

The Wazuh environment is hosted inside an Ubuntu virtual machine running on Oracle VirtualBox. Docker Compose orchestrates the deployment of the Wazuh containers, while the dashboard is accessed securely through HTTPS from the Windows host machine.

![Lab Architecture](screenshots/architecture-diagram.png)

### Components

| Component | Purpose |
|-----------|---------|
| Windows 11 Host | Used to access the Wazuh Dashboard through a web browser. |
| Oracle VirtualBox | Provides the virtualization environment. |
| Ubuntu 24.04 LTS | Hosts the Docker Engine and Wazuh services. |
| Docker Engine | Runs and manages all Wazuh containers. |
| Wazuh Dashboard | Web interface for monitoring and investigation. |
| Wazuh Manager | Collects and analyzes security events. |
| Wazuh Indexer | Stores and indexes security data for fast searching and visualization. |

---

## Lab Environment

| Component | Specification |
|-----------|---------------|
| Host Operating System | Windows 11 |
| Virtualization Platform | Oracle VirtualBox |
| Guest Operating System | Ubuntu 24.04 LTS |
| Container Platform | Docker Engine |
| Container Orchestration | Docker Compose |
| SIEM Platform | Wazuh 4.12.0 |
| Access Method | HTTPS via Ubuntu VM IP Address |

---

## Deployment Procedure

The deployment was performed using the official Wazuh Docker single-node environment. Docker Compose was used to orchestrate all required containers while Ubuntu served as the host operating system inside Oracle VirtualBox.

### 1. Clone the Official Repository

```bash
git clone https://github.com/wazuh/wazuh-docker.git
```

This downloads the official Wazuh Docker deployment files.

---

### 2. Navigate to the Single-Node Deployment

```bash
cd wazuh-docker/single-node
```

The single-node deployment is suitable for home labs, learning environments, and proof-of-concept deployments.

---

### 3. Generate SSL Certificates

Before starting the containers, SSL certificates were generated for secure communication between Wazuh components.

```bash
docker compose -f generate-indexer-certs.yml run --rm generator
```

---

### 4. Deploy the Containers

```bash
docker compose up -d
```

Docker Compose creates and starts:

- Wazuh Dashboard
- Wazuh Manager
- Wazuh Indexer

---

### 5. Verify the Deployment

The following commands were used to verify that all services were running correctly.

```bash
docker compose ps
```

```bash
docker ps
```

---

### 6. Access the Dashboard

Once all services were operational, the Wazuh Dashboard was accessed through the Ubuntu virtual machine using HTTPS.

```
https://<Ubuntu_VM_IP>
```

The default credentials for the deployment were:

```
Username : admin
Password : SecretPassword
```

> **Note:** The browser displays a security warning because Wazuh uses a self-signed SSL certificate in the lab environment.

---

## Deployment Verification

After deployment, multiple verification steps were performed to confirm that the environment was functioning correctly.

### Verification Checklist

- Docker containers started successfully.
- Wazuh Dashboard was accessible through HTTPS.
- All Wazuh services were operational.
- Docker Compose reported healthy containers.
- Remote access from the Windows host was successful.

### Deployment Evidence

### Login Page

![Login](screenshots/01-login-page.png)

---

### Dashboard

![Dashboard](screenshots/02-dashboard-home.png)

---

### Docker Compose Status

![Docker Compose](screenshots/03-docker-compose-ps.png)

---

### Running Containers

![Docker PS](screenshots/04-docker-ps.png)

---

### Remote Dashboard Access

![Remote Access](screenshots/05-remote-dashboard-access.png)

---

### Ubuntu Version

![Ubuntu](screenshots/06-ubuntu-version.png)

---

### Docker Version

![Docker](screenshots/07-docker-version.png)

---

### System Information

![System](screenshots/08-system-information.png)

---

### Project Structure

![Project](screenshots/09-directory-tree.png)

---

# Troubleshooting

During the deployment, several issues were encountered. Each problem was investigated systematically until the root cause was identified and resolved.

## Issue 1 — Docker Permission Denied

**Problem**

Docker commands failed because the current user did not have permission to communicate with the Docker daemon.

**Root Cause**

The user account was not a member of the Docker group.

**Resolution**

The user was added to the Docker group and the session was refreshed before continuing with the deployment.

---

## Issue 2 — Manager and Indexer Container Confusion

**Problem**

Administrative tools were executed inside the Wazuh Manager container instead of the Wazuh Indexer container.

**Root Cause**

The Wazuh password management utilities are available only inside the Indexer container.

**Resolution**

The correct container was identified and administrative commands were executed from the Wazuh Indexer container.

---

## Issue 3 — Password Tool Required Root Privileges

**Problem**

The password management tool returned the following message:

```
This script must be run as root.
```

**Root Cause**

The tool was executed using a non-root user inside the container.

**Resolution**

The Indexer container was accessed as the root user before running administrative utilities.

---

## Issue 4 — Dashboard Authentication Failure

**Problem**

Authentication failed when attempting to log into the Wazuh Dashboard.

**Root Cause**

The deployment was using the default credentials provided by the official Docker deployment.

**Resolution**

The correct credentials were used:

```
Username : admin
Password : SecretPassword
```

---

## Issue 5 — Dashboard Accessibility

**Problem**

Initially, the dashboard was accessed only from within the Ubuntu virtual machine.

**Root Cause**

The deployment needed to be verified from the Windows host system.

**Resolution**

The dashboard was successfully accessed remotely using the Ubuntu virtual machine's IP address over HTTPS.

---

## Key Takeaways

This deployment provided practical experience with:

- Docker container management
- Wazuh service architecture
- Docker Compose deployment
- Linux command-line troubleshooting
- Authentication troubleshooting
- Container-based administration
- System verification and validation

The troubleshooting process reinforced the importance of identifying the root cause of problems before applying changes, resulting in a stable and repeatable deployment.

---

# Lessons Learned

Building this lab provided practical experience beyond simply deploying a SIEM platform. Throughout the project, several concepts became much clearer through hands-on troubleshooting and validation.

### Technical Skills Gained

- Docker container deployment and management
- Docker Compose orchestration
- Wazuh SIEM architecture
- Ubuntu system administration
- Linux command-line troubleshooting
- HTTPS access using self-signed certificates
- Container-based service verification
- Basic infrastructure documentation

### Operational Understanding

This project reinforced the importance of:

- Verifying deployments rather than assuming success.
- Investigating root causes before making configuration changes.
- Understanding the responsibilities of each Wazuh component.
- Documenting troubleshooting steps for future reference.
- Maintaining structured deployment documentation.

---

# Future Enhancements

This deployment serves as the foundation for a larger SOC home lab.

The following projects are planned:

- Deploy Wazuh Agents on Linux and Windows
- Centralized log collection
- SSH brute-force detection
- File Integrity Monitoring (FIM)
- Vulnerability Detection
- Active Response
- MITRE ATT&CK mapping
- Custom detection rules
- Security incident investigations
- SOC case studies

---

# References

- Official Wazuh Documentation  
  https://documentation.wazuh.com/

- Wazuh Docker Repository  
  https://github.com/wazuh/wazuh-docker

- Docker Documentation  
  https://docs.docker.com/

---

# Author

**Rohan Paygude**

Cybersecurity Enthusiast | SOC Analyst Aspirant | Home Lab Builder

GitHub:
https://github.com/rohanpayGude
