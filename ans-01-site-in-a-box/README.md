# ANS-01 Site-in-a-box: Greenfield branch provisioning

Renders and pushes the base configuration that every Cisco IOS-XE device gets:
hostname, NTP, DNS, AAA/TACACS+, SNMPv3, syslog, banners, and SSH hardening.

## Before you start

1. Install the collections: `ansible-galaxy collection install -r requirements.yml`
2. Replace the placeholders in `inventory/group_vars/all/vault.yml`, then encrypt it:
   `ansible-vault encrypt inventory/group_vars/all/vault.yml`
3. Generate SSH keys on each device at day 0. The role doesn't do this, because
   `crypto key generate rsa` isn't idempotent:
   `crypto key generate rsa general-keys modulus 2048`

## Usage

Render configs to `build/` without touching devices, and review them:

    ansible-playbook site.yml -e base_push=false --ask-vault-pass

Preview changes against live devices, then apply:

    ansible-playbook site.yml --check --diff --ask-vault-pass
    ansible-playbook site.yml --ask-vault-pass

## Layout

Each section of the base config has its own template in
`roles/base/templates/base/`. `base.j2` includes them in a deliberate order:
the local admin account is created before the VTY access-class is applied.
