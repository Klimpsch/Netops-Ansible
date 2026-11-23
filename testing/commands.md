
# Dry run
ansible-playbook ntp-core.yml -k --check --diff

ansible-playbook ntp-core.yml --syntax-check
ansible-playbook site.yml --list-hosts     # which devices it would hit
ansible-playbook site.yml --list-tasks     # which tasks would run
