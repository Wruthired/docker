В первую очередь  устанавливаем linux на виртуалку , для этого рекомендовано +4 ядра, и +4 гига оперативки.
  После установки Linux переходим к командам 
  1)git clone https://github.com/skl256/grafana_stack_for_docker.git соглашаемся со всеми установками ![image](https://github.com/user-attachments/assets/3df0af5f-6014-4731-8690-644127578e32)
2)cd grafana_stack_for_docker
3)sudo mkdir -p /mnt/common_volume/swarm/grafana/config
4)sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data}
5)sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}
6)touch /mnt/common_volume/grafana/grafana-config/grafana.ini
7)cp config/* /mnt/common_volume/swarm/grafana/config/
8)mv grafana.yaml docker-compose.yaml
9)sudo yum install wget 
10)sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo скачиваем репозиторий 
