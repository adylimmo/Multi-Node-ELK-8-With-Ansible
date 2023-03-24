# Multi-Node-ELK-8-With-Ansible

- git clone https://github.com/adylimmo/Multi-Node-ELK-8-With-Ansible.git #clone repo
- cd Multi-Node-ELK-8-With-Ansible #masuk direktori
- ansible all -i hosts -m ping #pastikan semua node terhubung 
- ansible-playbook -i hosts 1.change_hostname.yml #ubah hostname
- ansible-playbook -i hosts 2.install_single_node.yml #install elasticsearch