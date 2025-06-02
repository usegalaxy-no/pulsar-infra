# Pulsar Ansible Playbook for usegalaxy.no

This repository contains an Ansible playbook to automate the installation and configuration of [Pulsar](https://github.com/galaxyproject/pulsar) on target hosts.

## What the Playbook Does

- Installs required Python packages and tools.
- Creates a dedicated `pulsar` user and group.
- Sets up the Pulsar installation directory and a Python virtual environment.
- Installs Pulsar (optionally with web components).
- Installs message queue dependencies (`kombu`, `pycurl`) if mq is enabled.
- Generates Pulsar configuration files.
- Sets up log directories.
- Deploys configuration templates (`server.ini`, `app.yml`).
- Installs and enables a systemd service for Pulsar.
- Configures Slurm DRMAA for job execution.

## Requirements

- **Pulsar and Slurm must be installed on the same node.**
- This Pulsar setup is designed to use Slurm as the job runner.

## How to Run

1. **Configure Inventory**

   Define your Pulsar nodes in your Ansible inventory file, e.g.:

   ```
   [pulsar_nodes]
   your.pulsar.host1
   your.pulsar.host2
   ```

2. **Vault Secrets**

   Ensure you have a `vault_staging.yaml` file with the required secrets (e.g., `vault_pulsar_mq`).

3. **Run the Playbook**

   ```
   ansible-playbook -i inventory staging.inv --ask-vault-pass
   ```

   - Replace `inventory` with your inventory file path.
   - Use `--ask-vault-pass` if your vault file is encrypted.

## What Will Happen

- Pulsar will be installed under `/opt/pulsar` using a Python virtual environment.
- Configuration files will be generated and placed in the install directory.
- A systemd service (`pulsar.service`) will be created and started.
- If enabled, message queue support will be configured using the provided AMQP URL.
- Slurm DRMAA will be installed and configured for job execution.

## Customization

You can adjust variables in `pulsar.yaml` to control:

- Whether to install web components (`install_web_components`)
- Whether to enable message queue support (`mq`)
- Installation paths and user/group names
- Slurm DRMAA version and configuration

## Templates

- `templates/server.ini.j2`: Main Pulsar server configuration.
- `templates/app.yml.j2`: Pulsar application configuration.
- `templates/pulsar.service.j2`: Systemd service template.

---
For more details, see the comments in `pulsar.yaml`.
