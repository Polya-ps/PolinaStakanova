# PolinaStakanova

Методичка 

Создаем виртуальную машину. В "образ ISO" на диске Student, в папке К-ИСП-49,выбираем папку Давыдов АА и там лежит документ.

При создании делаем процессов побольше, памяти.

Когда загрузилась машина мы прописываем команды 

1) git clone https://github.com/skl256/grafana_stack_for_docker.git &&
   после вписания этой команды спрашивают об установлении пакета [N/y] вибираем y

2) cd grafana_stack_for_docker &&

3) sudo mkdir -p /mnt/common_volume/swarm/grafana/config &&

4) sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data} &&

5) sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}

6) touch /mnt/common_volume/grafana/grafana-config/grafana.ini &&

7) cp config/* /mnt/common_volume/swarm/grafana/config/ &&

8) mv grafana.yaml docker-compose.yaml &&

9) docker compose up -d

Далее пишем другие команды
1) yum install curl
2) COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
3) curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
4) sudo chmod +x /usr/bin/docker-compose
5) sudo docker-compose --version

Далее добавляем следующие команды
1) sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
2) sudo yum install docker-ce docker-ce
3) sudo systemctl enable docker --now
4) sudo docker compose up -d
