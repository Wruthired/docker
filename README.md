В первую очередь  устанавливаем linux на виртуалку , для этого рекомендовано +4 ядра, и +4 гига оперативки.
  После установки Linux переходим к командам 
  1)git clone https://github.com/skl256/grafana_stack_for_docker.git соглашаемся со всеми установками 
  
  ![image](https://github.com/user-attachments/assets/3df0af5f-6014-4731-8690-644127578e32)
  
2)cd grafana_stack_for_docker
3)sudo mkdir -p /mnt/common_volume/swarm/grafana/config
4)sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data}
5)sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}
6)touch /mnt/common_volume/grafana/grafana-config/grafana.ini
7)cp config/* /mnt/common_volume/swarm/grafana/config/
8)mv grafana.yaml docker-compose.yaml
9)sudo yum install wget 
10)sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo скачиваем репозиторий 

![image](https://github.com/user-attachments/assets/38b29164-9643-48f2-afb3-7092d18f3d8b)

11)устанавливаем docker : sudo yum install docker-ce docker-ce-cli containerd.io

![image](https://github.com/user-attachments/assets/0f7c4b29-9d91-41f0-889d-0b3b1139cf9d)

(со всем соглашаемся и скачиваем)
12) sudo systemctl enable docker --now 
13) sudo yum install curl
14) COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
15)sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose

![image](https://github.com/user-attachments/assets/90c7148d-8fd4-40c6-b80c-9257dac06b7f)

16)sudo chmod +x /usr/bin/docker-compose
17) docker-compose --version (проверяем версию)
18) sudo docker compose up -d команда создает контейнера с правами супер пользователя 

![image](https://github.com/user-attachments/assets/b55a3ab1-aec8-47fd-9583-9d4576f615ed)


19) sudo vi docker-compose.yaml команда открывает файл в текстовом редакторе , вставляем туда команды: node-exporter: 
    image: prom/node-exporter 
    volumes: 
      - /proc:/host/proc:ro 
      - /sys:/host/sys:ro 
      - /:/rootfs:ro 
    container_name: exporter 
    hostname: exporter 
    command: 
      - --path.procfs=/host/proc 
      - --path.sysfs=/host/sys 
      - --collector.filesystem.ignored-mount-points 
      - ^/(sys|proc|dev|host|etc|rootfs/var/lib/docker/containers|rootfs/var/lib/docker/overlay2|rootfs/run/docker/netns|rootfs/var/lib/docker/aufs)($$|/) 
    ports: 
      - 9100:9100 
    restart: unless-stopped 
    environment: 
      TZ: "Europe/Moscow" 
    networks: 
      - default
20)переходим на сайт localhost:3000
логин и пароль admin admine
21) создаем Dashboard
22) жмем кнопку +Add visualizationа ,а затем "Configure a new data source"
23) connection http://prometheus:9090
24) Authentication потом Basic Authentication (admin admin)
    victoriametrics
25) 
