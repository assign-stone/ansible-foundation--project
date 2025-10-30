# 🧩 Ansible Foundation Project

Automating Configuration Management with Ansible on AWS

![Ansible Banner](https://cdn.jsdelivr.net/gh/assign-stone/ansible-foundation--project/assets/ansible-banner.gif)

---

## 📘 Overview

This repository contains an **Ansible automation setup** deployed and configured on an **AWS EC2 instance**.
It demonstrates step-by-step how to install, configure, and use **Ansible** to automate tasks such as server connectivity testing and Apache web server deployment.

---

## ⚙️ AWS Infrastructure Setup

For this project, **three EC2 instances** were launched within the **same VPC** and **availability zone** to simulate a production-like Ansible environment.

| Instance Name   | Role         | Description                                             |
| --------------- | ------------ | ------------------------------------------------------- |
| 🧑‍💻 `ansible` | Control Node | Executes Ansible playbooks and manages other instances. |
| 🖥️ `host-1`    | Managed Node | Target EC2 instance managed by Ansible.                 |
| 🖥️ `host-2`    | Managed Node | Second target instance for scalability and validation.  |

### Configuration Details

* **Region:** `us-east-1 (N. Virginia)`
* **Availability Zone:** `us-east-1b`
* **Instance Type:** `t3.medium`
* **Network:** All instances are in the same **VPC and subnet** for private IP communication.
* **Authentication:** Configured **SSH key-based access** from the control node to managed nodes.

📸 **Reference Screenshot:**
![EC2 Instances](Screenshot-1322.png)

---

## 📁 Project Structure

```
ansible/
├── ansible.cfg
├── apache.yml
├── aws/
│   └── key-ansible.pem
├── inventory/
│   └── hosts
├── playbooks/
│   ├── apache.yml
│   └── ping.yml
└── roles/
```

---

## ⚙️ 1. Environment Setup

### Update and Install Prerequisites

```bash
sudo yum update -y
sudo yum install python -y
sudo yum install tree -y
python --version
```

### Install Ansible

```bash
pip install ansible
sudo yum install pip -y
pip install ansible
sudo yum search ansible
sudo yum install ansible.noarch -y
ansible --version
```

### (Optional Alternative)

```bash
sudo dnf install ansible -y
```

---

## 📂 2. Directory & File Setup

### Create Required Folders

```bash
cd /etc/ansible
sudo mkdir aws inventory playbooks roles
```

### Create Configuration and Key Files

```bash
sudo touch ansible.cfg
sudo nano ansible.cfg
sudo touch aws/key-ansible.pem
sudo nano aws/key-ansible.pem
```

### Verify Folder Tree

```bash
tree
```

---

## 🌐 3. Inventory Configuration

Define managed hosts inside the `inventory/hosts` file:

```bash
sudo nano inventory/hosts
```

Example:

```
[dev]
<dev-server-ip> ansible_user=ec2-user ansible_ssh_private_key_file=aws/key-ansible.pem

[prod]
<prod-server-ip> ansible_user=ec2-user ansible_ssh_private_key_file=aws/key-ansible.pem
```

---

## 🧠 4. Testing Connectivity

Use Ansible ping module to verify SSH communication:

```bash
ansible -m ping all
ansible -m ping dev
ansible -m ping prod
```

---

## 🚀 5. Creating and Running Playbooks

### Example 1: Ping Playbook

Create `playbooks/ping.yml`:

```yaml
- name: Test server connectivity
  hosts: all
  tasks:
    - name: Ping all hosts
      ping:
```

Run:

```bash
ansible-playbook -i inventory/hosts playbooks/ping.yml
```

### Example 2: Apache Installation Playbook

Create `playbooks/apache.yml`:

```yaml
- name: Install and Start Apache
  hosts: all
  become: yes
  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present

    - name: Start Apache Service
      service:
        name: httpd
        state: started
        enabled: yes
```

Run:

```bash
ansible-playbook -i inventory/hosts playbooks/apache.yml
```

---

## 🧹 6. Moving Project to Home Directory

Since `/etc` requires root privileges, move your setup to the EC2 user’s home:

```bash
sudo mv /etc/ansible /home/ec2-user/ansible
sudo chown -R ec2-user:ec2-user /home/ec2-user/ansible
cd /home/ec2-user/ansible
```

---

## 💻 7. Git Setup and Version Control

### Initialize Repository

```bash
git init
git add .
git commit -m "Initial commit"
```

### Configure Git Identity (Optional)

```bash
git config --global user.name "Shivani Joshi"
git config --global user.email "your-email@example.com"
```

### Rename Default Branch

```bash
git branch -M main
```

### Add Remote Repository

```bash
git remote add origin https://github.com/assign-stone/ansible-foundation--project.git
```

### Push to GitHub

```bash
git push -u origin main
```

If the remote already has commits:

```bash
git pull origin main --allow-unrelated-histories
git push -u origin main
```

To permanently set merge behavior:

```bash
git config pull.rebase false
```

---

## ✅ 8. Verification

Check repository status:

```bash
git status
git log --oneline
```

Verify playbook execution:

```bash
ansible-playbook -i inventory/hosts playbooks/ping.yml
ansible-playbook -i inventory/hosts playbooks/apache.yml
```

---

## 📜 Summary

| Step | Task             | Command                                                  |
| ---- | ---------------- | -------------------------------------------------------- |
| 1    | Install Ansible  | `sudo yum install ansible -y`                            |
| 2    | Create Inventory | `sudo nano inventory/hosts`                              |
| 3    | Ping All Hosts   | `ansible -m ping all`                                    |
| 4    | Create Playbook  | `sudo nano playbooks/ping.yml`                           |
| 5    | Run Playbook     | `ansible-playbook -i inventory/hosts playbooks/ping.yml` |
| 6    | Push to GitHub   | `git push -u origin main`                                |

---

## 💡 Key Learnings

* How to install and configure Ansible on an EC2 instance
* How to define inventories and use SSH keys for authentication
* How to write and execute YAML playbooks
* How to use Git for version control and remote synchronization
* How to manage multiple EC2 hosts within a shared AWS VPC using Ansible

---

## 🤝 Contributing

We welcome contributions from the community!

If you’d like to **add new roles, improve documentation, or optimize playbooks**, follow these steps:

1. 🍴 **Fork** this repository
2. 🌿 **Create** a branch (`git checkout -b feature-name`)
3. 💻 **Commit** changes (`git commit -m 'Add new feature'`)
4. 🚀 **Push** to your fork and open a **Pull Request**

> All contributions are appreciated — from small improvements to new modules!

---

## 🪪 Author

**👩‍💻 Shivani Joshi**
📂 [GitHub Profile](https://github.com/assign-stone)

---

## 🌟 Show Your Support

If this project helped you,
⭐ **Star this repository** — it inspires me to share more automation projects!

---
