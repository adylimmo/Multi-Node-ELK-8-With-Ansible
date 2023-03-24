# Multi-Node-ELK-8-With-Ansible

- cd /home #masuk direktori home
- git clone https://github.com/adylimmo/Multi-Node-ELK-8-With-Ansible.git #clone repo
- cd Multi-Node-ELK-8-With-Ansible #masuk direktori hasil clone
- ansible all -i hosts -m ping #pastikan semua node terhubung 
- ansible-playbook -i hosts 1.change_hostname.yml #ubah hostname dan tambah /etc/hosts
- ansible-playbook -i hosts 2.install_single_node.yml #install elasticsearch
- ansible-playbook -i hosts 3.show_token_node1.yml #show token

# PASTIKAN TOKEN SUDAH DI TAMBAHKAN PADA NODE LAINNYA.

- /usr/share/elasticsearch/bin/elasticsearch-reconfigure-node --enrollment-token eyJ2ZXIiOiI4LjYuMiIsImFkciI6WyIxOTIuMTY4LjU2LjIzMDo5MjAwIl0sImZnciI6IjJlMjNmNTZjNzZhZmFkNTg4OTBmNWZjNzUyNDYzNTA1NGZiZGY4NGY1OWEyY2VlZDYzODQ4NDMwMzk0MTgzMjciLCJrZXkiOiJhVjFoRW9jQndCV19mXzJ1c08xQzpjLWtmdER4MFM0eXJUcDgwLW05eFdRIn0=
- ansible-playbook -i hosts 4.join-node.yml #merubah semua config cluster
- ansible-playbook -i hosts 5.restart_node.yml #restart all node


# JALANKAN PADA NODE 1

- /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
- curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic https://192.168.56.230:9200
- curl -k -XGET "https://192.168.56.230:9200/_cat/nodes?v" -u elastic
- curl -k -XGET "https://192.168.56.230:9200/_cat/health?v" -u elastic