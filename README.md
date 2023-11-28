# dockerapp-prometheusGrafana-monitoring

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/5c203f26-e025-4cd3-9b04-b283f7b61379)

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/1395279b-c97c-4c66-ab58-8ede2e023a60)

    sudo apt-get update -y
    sudo apt-get upgrade -y
    sudo apt-get install docker.io -y
    sudo apt-get install docker-compose -y

    mkdir mydockerapp
    cd mydockerapp
    mkdir backup
    sudo nano docker-compose.yml

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/66a0efa3-8936-4b35-9936-3ec00d8f089e)

    version: '3.3'
    
    services:
      mysql:
        image: mysql:8.0.26
        restart: always
        environment:
          MYSQL_ROOT_PASSWORD: root
        volumes:
          - /home/ubuntu/mysqldocker/backup:/var/lib/mysql
        ports:
          - "3306:3306"
    
    docker-compose up -d
    docker ps

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/a4f6be1b-a634-4d55-bb34-799f7ee2bdfc)

    cd /home/ubuntu/
    mkdir mytomcatapp
    cd mytomcatapp
    sudo nano docker-compose.yml

    version: '3'
    
    services:
      tomcat:
        image: tomcat:10-jdk17-openjdk-slim
        ports:
          - "8080:8080"
        volumes:
          - ./webapps:/usr/local/tomcat/webapps
          - ./logs:/usr/local/tomcat/logs
        restart: always


![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/c0a8f4bb-7d65-47af-943a-3fb55471b901)
  
    docker-compose up -d
    docker ps

  ![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/84e3d68c-ead0-40c7-9d64-45d6c46443f0)

    ##################Prometheus############################
    #Download the latest version of Prometheus using the following command:
    wget https://github.com/prometheus/prometheus/releases/download/v2.30.0/prometheus-2.30.0.linux-amd64.tar.gz
    #Extract the downloaded archive:
    tar xvfz prometheus-2.30.0.linux-amd64.tar.gz
    #Move into the extracted directory:
    cd prometheus-2.30.0.linux-amd64
    #Start Prometheus as a background service:
    ./prometheus --config.file=prometheus.yml &

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/349c6b6e-972b-4dec-b699-f8f71054839c)

    #####################Grafana############################\
    #Import the GPG key used by the Grafana package:
    wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
    #Add the Grafana repository to the APT sources:
    sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
    #Update the package lists:
    sudo apt update
    #Install Grafana:
    sudo apt install grafana
    #Start the Grafana service:
    sudo systemctl start grafana-server
    #Enable the Grafana service to start on system boot:
    sudo systemctl enable grafana-server

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/e251378b-6753-4ef1-9e7b-5a70b7085d28)

    cd /etc/docker
    sudo vi daemon.json

# these is daemon.json file content
    {
      "metrics-addr" : "0.0.0.0:9393",
      "experimental" : true
    }

    sudo systemctl stop docker.service
    sudo systemctl start docker.service

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/8379f34d-fd4f-43b1-b2d3-173a74ab2128)

cd /home/ubuntu/prometheus-2.30.0.linux-amd64
sudo vi prometheus.yml

    # my global config
    global:
      scrape_interval: 15s
      evaluation_interval: 15s
    
    # Alertmanager configuration
    alerting:
      alertmanagers:
        - static_configs:
            - targets:
              # - alertmanager:9093
    
    # Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
    rule_files:
      # - "first_rules.yml"
      # - "second_rules.yml"
    
    # A scrape configuration containing exactly one endpoint to scrape: Prometheus itself.
    scrape_configs:
      - job_name: "prometheus"
        static_configs:
          - targets: ["localhost:9090"]
    
      - job_name: "Docker Job"
        static_configs:
          - targets: ["Public IP:9393"]

Restart Prometheus

     ps aux | grep prometheus
     kill -SGTERM process-id

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/0641dd4b-3a5f-4119-b605-864fb9040ddd)

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/61eada4b-cb84-41d3-a0f8-b64b98e26e3e)

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/8b14f331-517d-42c7-8057-69ae33ce3886)

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/cd406b74-54f6-4880-8c3d-74198dbbdd17)

![image](https://github.com/kohlidevops/dockerapp-prometheusGrafana-monitoring/assets/100069489/12e4ae3c-3715-479b-a288-3498cec919b8)

