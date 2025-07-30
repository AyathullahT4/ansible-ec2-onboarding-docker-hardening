# ansible-ec2-onboarding-docker-hardening

🔧 EC2 Onboarding Automation with Docker Installation and Host Hardening  

This repository captures an Ansible-based automation workflow used to onboard EC2 instances by:

- Installing Docker Engine with proper version pinning
- Creating a dedicated user with Docker group membership
- Hardening the host: disabling root SSH login, enabling UFW, and opening only required ports
  
---

🧩 Why It Exists  
- Provisioning compute is only one part of onboarding. This setup ensures that even base-level hosts follow a minimal viable standard for operational readiness — right from the first login.

Helps in:

* Quickly preparing EC2 instances for Docker workloads
* Enforcing initial SSH and firewall hardening
* Avoiding repetitive manual provisioning tasks
* Applying tagging standards for visibility and cost governance

---

---

## 📁 Repository Structure

```
ansible-ec2-onboarding-docker-hardening/
├── README.md                    # Project documentation
├── group_vars/                 # Group-level variable definitions
├── host_vars/                  # Host-level variable definitions
├── inventory.ini               # Static inventory (can be replaced with dynamic)
├── roles/
│   ├── docker_install/         # Installs Docker and manages service
│   │   └── tasks/
│   │       └── main.yml
│   ├── security_hardening/     # Applies SSH hardening and UFW rules
│   │   ├── handlers/
│   │   │   └── main.yml        # Service restart handlers, if any
│   │   └── tasks/
│   │       └── main.yml
│   └── users/                  # Creates users, sets up keys and sudo
│       └── tasks/
│           └── main.yml
├── site.yml                    # Main playbook that includes all roles

```
---

## ⚙️ What It Covers

✅ Docker installation with service enabled

✅ Creation of a non-root user for operations with sudo access

✅ SSH key injection for passwordless login

✅ Disable root login over SSH

✅ Enable UFW firewall and restrict incoming traffic

✅ Allow only required ports (SSH, Docker)

✅ Use of Ansible tags for modular runs

✅ Supports idempotent runs on Ubuntu-based EC2s

---

🛠️ Tools & Techniques Used  
- **Ansible** with layered roles
- **SSH over inventory.ini** (static IPs for test setup)
- **UFW & SSH configs** for access control
- **Docker CE** install via official convenience script (with checksum validation and lock-in)

---

🔁 Can Be Extended To  
- Auto-register new EC2s via dynamic inventory or ASG discovery  
- Add CIS benchmark checks using tools like Lynis or OpenSCAP  
- Integrate Docker image signing or runtime policies  
- Setup audit logging for user and container actions  

---

## ✅ Example Usage

Run the full onboarding:

```bash
ansible-playbook -i inventory.ini site.yml
```

Run only security hardening tasks:

```bash
ansible-playbook -i inventory.ini site.yml --tags security
```

---

## 🏷️ Tag Structure

Each role defines tags for better targeting:

* `docker` – Docker installation
* `user` – EC2 user setup
* `security` – Hardening tasks

---

## 💡 Notes

* Designed for initial EC2 provisioning — doesn't interfere with existing configs beyond what it manages
* Ideal for teams maintaining hardened golden AMIs or fleet bootstrap steps
* Keeps changes minimal, traceable, and reusable across environments

---

📌 Maintainer Notes

 Use this repository as a starting point or reusable template for onboarding automation across environments. It's structured for readability and modular execution — one role, one responsibility.
