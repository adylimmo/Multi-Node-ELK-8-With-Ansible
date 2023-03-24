# Multi-Node-ELK-8-With-Ansible

- cd /home
- git clone https://github.com/adylimmo/Multi-Node-ELK-8-With-Ansible.git #clone repo
- cd Multi-Node-ELK-8-With-Ansible #masuk direktori
- ansible all -i hosts -m ping #pastikan semua node terhubung 
- ansible-playbook -i hosts 1.change_hostname.yml #ubah hostname
- ansible-playbook -i hosts 2.install_single_node.yml #install elasticsearch
- ansible-playbook -i hosts 3.show_token_node1.yml #show token