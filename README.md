# LW-Project-07-Ansible-Driven-Configuration-Management-Inside-Kubernetes-Pod-apache

---

# 🔧 Apache Web Server Automation via Ansible in Kubernetes

This project demonstrates a **simple and effective automation workflow** where an **Ansible Pod inside a Kubernetes cluster** remotely connects via SSH to another **Apache Pod** and updates the content served on the Apache web server — **without restarting the service**.

---

## 📌 Project Overview

**Goal:**
Deploy an Apache HTTP server container inside Kubernetes that can be configured remotely using an Ansible pod. The Ansible pod connects via SSH to update files inside the Apache pod — showcasing how remote configuration and content management can be achieved dynamically inside a cluster.

---

## 🧱 Components

| **Component**                 | **Description**                                                                                             |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `Dockerfile.target-pod`       | Builds the Apache pod image with OpenSSH and Apache HTTP Server. Serves HTML content from `/var/www/html/`. |
| `Dockerfile.ansible-pod`      | Builds the Ansible control pod image with Python, SSH client, and Ansible installed.                        |
| `index.html`                  | Placeholder HTML file to be copied into the Apache pod’s document root (`/var/www/html/`).                  |
| `ssh-target-pod-service.yaml` | Exposes the Apache pod inside the Kubernetes cluster (NodePort or ClusterIP).                               |
| `ansible-pod.yaml`            | Deploys the Ansible pod to the cluster to act as a remote configuration manager.                            |
| `inventory.ini`               | Ansible inventory file specifying the Apache pod’s SSH-accessible IP.                                       |
| `playbook.yaml`               | Ansible playbook that copies the `index.html` file into the Apache pod using SSH.                           |


---

## ⚙️ How It Works

1. **Build & Tag Docker Images:**

   * `Dockerfile.target-pod`: Creates a custom Apache image with SSH enabled.
   * `Dockerfile.ansible-pod`: Prepares an Ansible-ready image with SSH client and Python.
   * These images are built locally and used in their respective Kubernetes manifests.

2. **Deploy Apache Pod (Target Pod):**

   * Using `ssh-target-pod-service.yaml`, the Apache pod is deployed with ports **80 (HTTP)** and **22 (SSH)** exposed.
   * The pod serves `index.html` via Apache and accepts SSH connections for remote configuration.

3. **Deploy Ansible Pod (Control Pod):**

   * `ansible-pod.yaml` launches an Ansible control pod inside the cluster.
   * This pod includes all tools required to SSH and run Ansible playbooks remotely.

4. **Remote Configuration via Ansible:**

   * The Ansible pod uses `inventory.ini` to target the Apache pod’s IP.
   * `playbook.yaml` runs `ansible.builtin.copy` to overwrite the existing `index.html` file inside `/var/www/html/` of the Apache pod.

5. **Live Content Update (No Restart Needed):**

   * Since Apache automatically serves static files, the content change is **instantly reflected**.
   * You can verify it by accessing the pod IP or NodePort via browser or `curl`.

---



## 🚀 Setup Instructions



---

## 📁 Project Structure

```bash
.
├── ansible
│   ├── files
│   │   └── index.html
│   ├── inventory.ini
│   └── playbook.yaml
├── kubernetes
│   ├── ansible-pod.yaml
│   ├── Dockerfiles
│   │   ├── Dockerfile.ansible-pod
│   │   └── Dockerfile.target-pod
│   └── ssh-target-pod-service.yaml
├── Output-Images
└── README.md
```
