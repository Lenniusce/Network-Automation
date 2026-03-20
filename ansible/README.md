Get Environment Started

1) Start Netbox VM
2) Start Ansible VM
3) SSH to Ansible VM from VSCode Remote SSH
4) pyenv to python 3.12.3
5) cd to network-automation
6) pyenv prefix
7) pyenv local 3.12.3
8) source ~/.bashrc
9) python3 --version

Test out connection from Ansible to Netbox

curl -H "Authorization: Token XRrqxGPnbQ7qZjv9KpDv0fPmZk6xMUtnPSjOOJbJ" http://192.168.161.128:8000/api/status/

If it doesnt work make sure the tokens are set:

export NETBOX_API=<YOUR_NETBOX_URL> (note - must include http:// or https://)
export NETBOX_TOKEN=<YOUR_NETBOX_API_TOKEN>

Run these to see if inventory shows:

ansible-inventory -i netbox_inv.yml --graph

ansible-inventory -i netbox_inv.yml --list