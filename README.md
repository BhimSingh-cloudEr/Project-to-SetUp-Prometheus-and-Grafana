# Project to SetUp Prometheus and Grafana

### Virtual Machine 
![Image](https://github.com/user-attachments/assets/f3d94379-a1ab-41b8-b191-3a789d6470db)
### Application Access through ip_address_of_main_vm:8080
![Image](https://github.com/user-attachments/assets/2ac3f12c-0b5c-44eb-bbe3-f406a45ebca9)
### Node Exporter Tool Access through ip_address_of_main_vm:9100
![Image](https://github.com/user-attachments/assets/bbb148b5-daf0-4d44-a238-fe8578565efa)
### Blackbox Prometheus Tool Access through ip_address_of_grafana_vm:9115	
![Image](https://github.com/user-attachments/assets/913dd381-f73b-46b4-a2cc-83d2c6ae697c)
### Grafana Tool Access through ip_address_of_grafana_vm:3000
![Image](https://github.com/user-attachments/assets/a2dfa0bc-631b-40c5-ae87-1f6fd33662f1)
### CLI Visualization
![Image](https://github.com/user-attachments/assets/fe9df5ae-1dbe-4425-b36f-b9834fb99789)
![Image](https://github.com/user-attachments/assets/ac083174-9753-426a-bceb-6633101626d1)

## Prerequisites Tools  And these are the command which are run on Main instance
	--> Lunch 2 Instances of t2.medium size
	--> Connect them and update those server 
		--> sudo apt-get update
		--> java --version
		--> apt install openjdk-17-jre-headless 
		--> cd /home/ubuntu/
## Clone the repository from the DevOps-Shack_GitHub
	### git clone https://github.com/jaiswaladi246/Task-Master-Pro.git
		--> ls
		--> sudo apt-get install maven -y
		--> cd Task-Master-Pro
		--> mvn package
					As Look Like
					[INFO] Scanning for projects...
					[INFO]
					[INFO] ---------------------< com.example.todo:todo-app >----------------------
					[INFO] Building todo-app 1.0-SNAPSHOT
					[INFO] --------------------------------[ jar ]---------------------------------
					Downloading from central: https://repo.maven.apache.org/maven2/
		--> ls
		--> cd targets
		--> java -jar todo-app-1.0-SNAPSHOT.jar & 
				--> "&" run application in backgroung
		--> cd ../..  
					--> ( Come back to /ubuntu/ dir and download  node_exporter )
		-->  wget https://github.com/prometheus/node_exporter/releases/download/v1.9.0/node_exporter-1.9.0.linux-amd64.tar.gz
					--> node_exporter-1.9.0.linux-amd64.tar.gz  ( Look Like This )
		-->  ls
				Task-Master-Pro  node_exporter-1.9.0.linux-amd64.tar.gz
				root@ip-172-31-32-148:/home/ubuntu# tar -xvf node_exporter-1.9.0.linux-amd64.tar.gz
							node_exporter-1.9.0.linux-amd64/
							node_exporter-1.9.0.linux-amd64/LICENSE
							node_exporter-1.9.0.linux-amd64/NOTICE
							node_exporter-1.9.0.linux-amd64/node_exporter
				root@ip-172-31-32-148:/home/ubuntu# ls
							Task-Master-Pro  node_exporter-1.9.0.linux-amd64  node_exporter-1.9.0.linux-amd64.tar.gz
				root@ip-172-31-32-148:/home/ubuntu# rm node_exporter-1.9.0.linux-amd64.tar.gz
				root@ip-172-31-32-148:/home/ubuntu# ls
							Task-Master-Pro  node_exporter-1.9.0.linux-amd64
				root@ip-172-31-32-148:/home/ubuntu# mv node_exporter-1.9.0.linux-amd64/ node_exporter
				root@ip-172-31-32-148:/home/ubuntu# ls
							Task-Master-Pro  node_exporter
				root@ip-172-31-32-148:/home/ubuntu# cd node_exporter/
				root@ip-172-31-32-148:/home/ubuntu/node_exporter# ls
							LICENSE  NOTICE  node_exporter
				root@ip-172-31-32-148:/home/ubuntu/node_exporter# ./node_exporter
				
### Commands run on Grafana Instance
	-->  sudo apt-get update
	--> cd /home/ubuntu/
	--> ls
	--> pwd
	--> wget https://github.com/prometheus/prometheus/releases/download/v3.2.0/prometheus-3.2.0.linux-amd64.tar.gz
			--> To Download Prometheus
	--> ls
	--> tar -xvf prometheus-3.2.0.linux-amd64.tar.gz
	--> rm prometheus-3.2.0.linux-amd64.tar.gz
	--> mv prometheus-3.2.0.linux-amd64/ prometheus
	--> cd prometheus/
	--> ./prometheus & 
			--> To run the prometheus
	-->  vi prometheus.yml

### Below the commands are run sequencally

## Add these code to prometheus.yml in prometheus dir
 - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"

    static_configs:
      - targets: ['15.206.116.116:9100']

  - job_name: blackbox
    metrics_path: /probe
    params:
      module:
        - http_2xx
    static_configs:
      - targets:
          - https://github.com/jaiswaladi246/Task-Master-Pro
          - http://15.206.116.116:8080
    relabel_configs:
      - source_labels:
          - __address__
        target_label: __param_target
      - source_labels:
          - __param_target
        target_label: instance
      - target_label: __address
        replacement: 13.233.107.112:9115
		
### To download Grafana
	--> sudo apt-get install -y adduser libfontconfig1 musl
	--> wget https://dl.grafana.com/enterprise/release/grafana-enterprise_11.5.2_amd64.deb
	--> sudo dpkg -i grafana-enterprise_11.5.2_amd64.deb
	--> NOT starting on installation, please execute the following statements to configure grafana to start automatically using systemd
			--> sudo /bin/systemctl daemon-reload
			--> sudo /bin/systemctl enable grafana-server
					--> You can start grafana-server by executing
			--> sudo /bin/systemctl start grafana-server
	
	
### Prometheus Blackbox Exporter  & Node Exporter
[ref ]  - https://grafana.com/grafana/dashboards/7587-prometheus-blackbox-exporter/
### To Download Grafana
[ref]   - https://grafana.com/grafana/download?platform=linux
### To check your yml code correct or not visit this site
[ref]  - https://www.yamllint.com/

 
	