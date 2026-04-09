# VolPE Deployment

This setup automates the deployment of VolPE across multiple machines using Ansible. It handles installing Podman, configuring workers, and starting the gRPC services.

## Prerequisites

```bash
pip install ansible
git clone <your-repo-url>
cd volpe-ansible
```

### 1. Configure Inventory

Open `inventory.yml` and update it with your node details.

**Example Configuration:**
```yaml
all:
  vars:
    ansible_user: ubuntu
    ansible_ssh_private_key_file: ~/.ssh/id_rsa

master-node:
  ansible_host: 192.168.1.10

worker-1:
  ansible_host: 192.168.1.11
  worker_id: w1
  worker_memory_gb: 4.0
  worker_cpu_count: 4

worker-2:
  ansible_host: 192.168.1.12
  worker_id: w2
  worker_memory_gb: 2.0
  worker_cpu_count: 2
```

### 2. Verify SSH Access

Ansible requires passwordless SSH access to all nodes. Test your connection:

```bash
ssh user@<node-ip>
```

To set up keys:

```bash
ssh-keygen
ssh-copy-id user@<node-ip>
```

## Confirm SSH works
```ansible -i inventory.yml all -m ping```

## Deployment

### Dry Run
```bash
ansible-playbook -i inventory.yml site.yml --check
```

### Run Deployment
```bash
ansible-playbook -i inventory.yml site.yml
```

## Verification

Check if the services are up:

- **UI / Static Page:** `http://<master-ip>:8000/static`
- **Worker Check:**
```bash
curl http://<master-ip>:8000/workerCount
```

## Cleanup

To stop and remove everything:
```bash
ansible-playbook -i inventory.yml site.yml --tags teardown
```
