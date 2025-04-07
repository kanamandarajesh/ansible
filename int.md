Here are **detailed answers** to both **basic and scenario-based Ansible interview questions**, ideal for DevOps roles involving automation with Jenkins, AWS, Terraform, and more.

---

## ✅ **Basic Ansible Interview Questions — With Answers**

---

### **1. What is Ansible and how does it work?**
**Answer:**  
Ansible is an open-source configuration management and automation tool. It uses SSH to connect to managed nodes and executes **modules** written in Python or other languages. It’s **agentless**, so no software needs to be installed on the client machines.

---

### **2. What are inventory files in Ansible?**
**Answer:**  
An inventory is a list of hosts or groups of hosts on which Ansible runs tasks. It's usually located at `/etc/ansible/hosts` or can be passed with `-i`. You can use **static inventories (INI/YAML)** or **dynamic inventories** (scripts or plugins for AWS, Azure, etc.).

---

### **3. What is a playbook?**
**Answer:**  
A YAML file defining a set of plays (tasks mapped to hosts). Each play targets a group of hosts and executes a sequence of **tasks** using **modules** like `apt`, `yum`, `copy`, etc.

---

### **4. Difference between `roles` and `tasks`?**
**Answer:**
- **Tasks**: Basic units of execution inside a playbook.
- **Roles**: A standardized way of organizing tasks, variables, files, templates, and handlers to make playbooks reusable and modular.

---

### **5. How do you pass variables in Ansible?**
**Answer:**
- In playbooks (`vars`)
- In inventory files (`host_vars`, `group_vars`)
- Via `--extra-vars` (`-e`)
- From facts or registered variables

---

### **6. What are facts in Ansible?**
**Answer:**  
Facts are system-level variables gathered from target machines using `setup` module (e.g., `ansible_hostname`, `ansible_os_family`).

---

### **7. Difference between `when`, `register`, and `with_items`?**
- `when`: Conditional execution
- `register`: Stores output of a task for use later
- `with_items`: Loop through a list of items

---

### **8. How do you handle secrets in Ansible?**
**Answer:**  
Use **Ansible Vault** to encrypt secrets:
```bash
ansible-vault encrypt secrets.yml
```
You can decrypt during execution using `--ask-vault-pass` or `--vault-password-file`.

---

### **9. Difference between `ansible` and `ansible-playbook`?**
- `ansible`: Ad-hoc commands
- `ansible-playbook`: Executes YAML-based playbooks

---

### **10. What is idempotency?**
**Answer:**  
It means running the same playbook multiple times will produce the **same result**, without causing repeated changes. Ansible modules are idempotent by design.

---

## ✅ **Scenario-Based Ansible Interview Questions — With Answers**

---

### **1. Install a package only if not already installed**
```yaml
- name: Install nginx
  ansible.builtin.yum:
    name: nginx
    state: present
```
**Explanation:** The `state: present` ensures it's installed only if not already there.

---

### **2. Playbook fails on one host — how to debug?**
- Run with `-vvv` for verbose output.
- Limit execution: `--limit problematic-host`
- Use `ansible-playbook play.yml --step` to confirm step-by-step.
- Check facts, host reachability, and connectivity.

---

### **3. Reboot server and wait for it to come back**
```yaml
- name: Reboot and wait
  hosts: all
  tasks:
    - name: Reboot the machine
      reboot:

    - name: Wait for the machine to come back
      wait_for_connection:
        timeout: 300
```

---

### **4. Use dynamic inventory with AWS EC2**
- Use `aws_ec2` plugin or `ec2.py` script from `ansible-contrib`
- Example:
```ini
plugin: aws_ec2
regions:
  - us-east-1
filters:
  tag:Environment: dev
```

---

### **5. Copy a file only if changed**
```yaml
- name: Copy config
  copy:
    src: nginx.conf
    dest: /etc/nginx/nginx.conf
    owner: root
    mode: 0644
```
**Ansible only copies if checksum differs.**

---

### **6. Run playbook on a specific host or group**
```bash
ansible-playbook playbook.yml -l webservers
```
or
```yaml
- hosts: app1.example.com
```

---

### **7. Continue play even if task fails**
```yaml
- name: Try to stop service
  service:
    name: dummy
    state: stopped
  ignore_errors: yes
```

---

### **8. Run command only if file exists**
```yaml
- name: Run if file exists
  shell: echo "Running script"
  when: ansible_facts['os_family'] == "Debian" and lookup('file', '/tmp/myfile.txt', errors='ignore') != ""
```

---

### **9. Pass secret password securely**
- Store in **Vault**
```bash
ansible-vault encrypt passwords.yml
```
- Use in playbook:
```yaml
vars_files:
  - passwords.yml
```

---

### **10. Structure a large Ansible project**
```text
production/
staging/
group_vars/
  all.yml
  webservers.yml
host_vars/
roles/
  webserver/
    tasks/
    handlers/
    templates/
    files/
playbooks/
  site.yml
  deploy.yml
```
Use `ansible.cfg` to set default inventory, roles path, etc.

---

Want a **hands-on example of an Ansible playbook** with dynamic inventory, secret management, or service validation? I can whip one up!
