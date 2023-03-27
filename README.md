# Multi-Node-ELK-8-With-Ansible

- cd /home #masuk direktori home
- git clone https://github.com/adylimmo/Multi-Node-ELK-8-With-Ansible.git #clone repo
- cd Multi-Node-ELK-8-With-Ansible #masuk direktori hasil clone
- ansible all -i hosts -m ping #pastikan semua node terhubung 
- ansible-playbook -i hosts 1.change_hostname.yml #ubah hostname dan tambah /etc/hosts
- ansible-playbook -i hosts 2.install_single_node.yml #install elasticsearch
- ansible-playbook -i hosts 3.show_token_node1.yml #show token

# PASTIKAN TOKEN SUDAH DI TAMBAHKAN PADA NODE LAINNYA.

- /usr/share/elasticsearch/bin/elasticsearch-reconfigure-node --enrollment-token eyJ2ZXIiOiI4LjYuMiIsImFkciI6WyIxOTIuMTY4LjU2LjIzNDo5MjAwIl0sImZnciI6IjZhYjkwMDNiMzQ0ZDVkOTE3OTEzYmU5YzliYmM5NDEzMGRjMzBhZDJjNjY5YjFiMWQyZDQ3YWJjOGQ0MjNjNDUiLCJrZXkiOiJSSXd6SVljQnp1dmFwNHNyaEZUYzpRQkxaaUlxVFFXNnhNdEJjMVlBOXR3In0=
- ansible-playbook -i hosts 4.join-node.yml #merubah semua config cluster
- ansible-playbook -i hosts 5.restart_all_node.yml #restart all
- ansible-playbook -i hosts 6.restart_node.yml #restart node 2 dan node 3


# PASSWORD ELASTIC

- /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
- ' Password for the [elastic] user successfully reset.
New value: SxXWmPDRGtT0qGzI5egb '

# JALANKAN PADA NODE 1

- curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic https://192.168.56.234:9200
- curl -k -XGET "https://192.168.56.235:9200/_cat/nodes?v" -u elastic
- curl -k -XGET "https://192.168.56.236:9200/_cat/health?v" -u elastic


# TAMPILAN PADA BROWSER

<img src="Screenshot 2023-03-24-151024.png" alt="Alt text" title="Optional title">






#  INSTALL KIBANA DI NODE 2

- https://www.elastic.co/guide/en/kibana/current/deb.html
- apt install kibana -y
```
# Kibana config kibana.yml
server.port: 5601
server.host: "0.0.0.0"
```
- systemctl enable kibana && systemctl start kibana && systemctl restart kibana
- /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana 
``` eyJ2ZXIiOiI4LjYuMiIsImFkciI6WyIxOTIuMTY4LjU2LjIzNTo5MjAwIl0sImZnciI6IjZhYjkwMDNiMzQ0ZDVkOTE3OTEzYmU5YzliYmM5NDEzMGRjMzBhZDJjNjY5YjFiMWQyZDQ3YWJjOGQ0MjNjNDUiLCJrZXkiOiItQmRISVljQjhSTjhyY1ZoVWpCbTo0b0pRN1BZalFSRzlQT3pDLU02QjRBIn0=
```


# INSTALL METRICBEAT FILE

- wget https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-8.6.2-linux-x86_64.tar.gz
- tar -xvf metricbeat-8.6.2-linux-x86_64.tar.gz
- cd metricbeat-8.6.2-linux-x86_64
```
#######edit file metricbeat.yml
setup.kibana:
  host: "http://192.168.56.231:5601"
  ssl.verification_mode: "none"

output.elasticsearch:
  hosts: ["https://192.168.56.230:9200"]
  protocol: "https"
  #api_key: "id:api_key"
  username: "elastic"
  password: "UBu85_4DiQusVaHSjKC4"
  ssl.verification_mode: "none"
#######edit file metricbeat.yml
```
- ./metricbeat -e
- ./metricbeat setup --dashboards

# INSTALL METRICBEAT NODE 2

- sudo apt-get update && sudo apt-get install metricbeat -y

```
#######edit file metricbeat.yml
setup.kibana:
  host: "http://192.168.56.231:5601"
  ssl.verification_mode: "none"

output.elasticsearch:
  hosts: ["https://192.168.56.230:9200"]
  protocol: "https"
  #api_key: "id:api_key"
  username: "elastic"
  password: "UBu85_4DiQusVaHSjKC4"
  ssl.verification_mode: "none"
#######edit file metricbeat.yml
```

- sudo systemctl enable metricbeat && sudo systemctl start metricbeat
- sudo metricbeat -e
- sudo metricbeat setup --dashboards
- metricbeat test output
