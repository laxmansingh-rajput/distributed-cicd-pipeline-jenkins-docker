# Establish Connection Between Master and Agent

Jenkins follows a **Master–Agent (Controller–Agent)** architecture to distribute build and deployment workloads across multiple machines. This improves scalability, isolates heavy builds, and supports different platforms. Below are the steps to connect a Jenkins Agent to the Master using **SSH**.

---

## Prerequisites

### 1. Ensure Agent Is Ready

Make sure the agent machine has:
- Java installed
- Docker installed (if required)
- SSH service enabled and running

---

## Setup Steps

### 2. Generate SSH Key on Jenkins Master

On the Jenkins Master machine, generate an SSH key pair:

```bash
ssh-keygen -t rsa -b 4096
```

- Press **Enter** to accept the default location (`~/.ssh/id_rsa`)
- Do not set a passphrase (recommended for Jenkins automation)

This generates:
- **Private key:** `~/.ssh/id_rsa`
- **Public key:** `~/.ssh/id_rsa.pub`

### 3. Add Public Key to Agent (authorized_keys)

Login to the agent machine and ensure the `.ssh` directory exists:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Now, copy the public key content from the Jenkins Master and append it to the agent's `authorized_keys` file:

```bash
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

Set correct permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

This allows passwordless SSH access from Master to Agent.

### 4. Verify SSH Connectivity

From the Jenkins Master, verify SSH access to the agent:

```bash
ssh <username>@<agent-ip>
```

If login works without asking for a password, SSH setup is successful.

### 5. Add SSH Credentials in Jenkins

1. Go to **Manage Jenkins → Credentials**
2. Click **Add Credentials**
3. Choose **SSH Username with private key**
4. Configure:
   - **Username:** agent user
   - **Private Key:** paste the contents of `~/.ssh/id_rsa`
   - **ID:** (optional) `agent-ssh-key`

### 6. Create Agent Node

1. Go to **Manage Jenkins → Nodes → New Node**
2. Enter a node name (e.g., `agent1`)
3. Select **Permanent Agent**
4. Choose **Launch agent via SSH**
5. Provide:
   - **Host:** agent private IP
   - **Credentials:** SSH credentials added earlier
   - **Labels:** `agent1`
   - **Remote root directory:** `/home/<username>/jenkins`
6. Save the configuration

### 7. Verify Connection

- Agent status should show **Connected**
- Jenkins Master can now schedule jobs on the agent

