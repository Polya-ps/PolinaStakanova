# Polina Stakanova

Перед началом установки, нужно установить Linux Oracle на VirtualBox, для этого выполним ряд действий:

Создаем виртуальную машину. В "образ ISO" на диске Student, в папке К-ИСП-49,выбираем папку Давыдов АА и там лежит документ.

Иметь образ Linux, добавить побольше ядер и опертивной паямяти.

## **Далее переходим к установке docker с использованием grafana, вводим следующий набор команд:**

1) `git clone https://github.com/skl256/grafana_stack_for_docker.git `
   после вписания этой команды спрашивают об установлении пакета [N/y] вибираем y

![image](https://github.com/user-attachments/assets/a094b1e1-fbae-4aff-ad44-a19c78b5d4e1)

3) `cd grafana_stack_for_docker`
   
![image](https://github.com/user-attachments/assets/936bb14a-6479-4ea1-a7b2-bd6087586682)
 
3) `sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

![image](https://github.com/user-attachments/assets/e8c8078e-1cf6-4019-8136-2af990990800)

4) `sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data} `

![image](https://github.com/user-attachments/assets/6a9bd0fc-2e22-4b64-a6d4-131036525bde)

5) `sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana} && \
touch /mnt/common_volume/grafana/grafana-config/grafana.ini && \ `

![image](https://github.com/user-attachments/assets/126e9fe3-bf9f-4a42-8884-1f38a7372c4c)

6) touch /mnt/common_volume/grafana/grafana-config/grafana.ini 

![image](https://github.com/user-attachments/assets/45042295-f2ab-40cd-a2ef-634106663b1b)

8) cp config/* /mnt/common_volume/swarm/grafana/config/ 

![image](https://github.com/user-attachments/assets/8f05b63c-c981-411b-a943-91f91fac8e07)

9) mv grafana.yaml docker-compose.yaml 

![image](https://github.com/user-attachments/assets/0b54a2eb-2a08-47f1-bb4f-b80e08d039a6)

10)  sudo docker compose up -d

![image](https://github.com/user-attachments/assets/0c570ab8-a834-451d-a2a9-01df103301cc)

![image](https://github.com/user-attachments/assets/39bf2dfa-9e11-4095-8283-0710cab17667)

**Устанавливаем последнею версию и утилитю docker-compose**

1) `sudo yum install curl`


![image](https://github.com/user-attachments/assets/eb9a4a4f-72aa-4a9d-98a2-e95d0a903aa3)

2) `COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

![image](https://github.com/user-attachments/assets/4be240f5-bd9f-4cd8-b977-f191e75b9062)

3) `curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`

![image](https://github.com/user-attachments/assets/b0162140-ec0b-4cd6-b76f-e6955e71744c)

4) `sudo chmod +x /usr/bin/docker-compose`

5) `sudo docker-compose --version`

![image](https://github.com/user-attachments/assets/0b0f5862-e030-4dc3-bdc6-e4e6d5f8e5b1)

## **Установка и настройка Docker**

1) `sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
`
![image](https://github.com/user-attachments/assets/f8041f63-d355-46f9-8208-6e40a871597d)

2) ` sudo yum install docker-ce docker-ce`

![image](https://github.com/user-attachments/assets/a51b6314-c1cf-4dc2-8a11-fa2f73299701)

![image](https://github.com/user-attachments/assets/c8756496-b0e2-47a1-b168-cd00747e5bde)

3) `sudo systemctl enable docker --now`

4) `sudo docker compose up -d`

## **После установки Docker пишем sudo vi docker-compose.yaml**

_Нас перекинет в текстовый редактор, где:_

- Чтобы изменить что-то в редакторе тегов, нажмите insert на клавиатуре

- Чтобы сохранить что-то в этом документе, нажмите Esc и введите :wq! В этом текстовом редакторе мы должны поставить node-exporter после services
```
 node-exporter:
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
```
_**В паке config прописан весь код docker-compose.yaml**_

**Далее прописываем:**

`cd /mnt/common_volume/swarm/grafana/config `

`sudo vi prometheus.yaml`

- исправить targets: _**на exporter:9100**_

## **Grafana**

1) Заходим на сайт **Local host:3000**

- Через **admin**

2) На сайте заходим в Dashboards, следом в Create Dashboard, следом Add visualization, следом Configure a new date source.

- Из предложенных _**выбираем Prometheus**_ после этого прописываем **_в строке Connection_ http://prometheus:9090**

3) В строке Authentication выбираем из предложенных Basic authentication, следом заходим через имя admin, пароль admin

- Дальше нажимаем _**save+test**_
 
4) В меню выбираем вкладку Dashboards и создаем Dashboard
   
- ждем кнопку "Import"   
- Find and import dashboards for common applications at grafana.com/dashboards: 1860  //ждем кнопку Load
Select Prometheus

![image](https://github.com/user-attachments/assets/c837ac4e-59f8-492a-a0f4-439be2074df1)

## **VictoriaMetrics**

1) Для начала изменим docker-compose.yaml:

`cd grafana_stack_for_docker`

`sudo vi docker-compose.yaml`

2) После чего в в самом текстовом редакторе после prometheus вставляем
```
   vmagent:
    container_name: vmagent
    image: victoriametrics/vmagent:v1.105.0
    depends_on:
      - "victoriametrics"
    ports:
      - 8429:8429
    volumes:
      - vmagentdata:/vmagentdata
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    command:
      - "--promscrape.config=/etc/prometheus/prometheus.yml"
      - "--remoteWrite.url=http://victoriametrics:8428/api/v1/write"
    restart: always
  # VictoriaMetrics instance, a single process responsible for
  # storing metrics and serve read requests.
  victoriametrics:
    container_name: victoriametrics
    image: victoriametrics/victoria-metrics:v1.105.0
    ports:
      - 8428:8428
      - 8089:8089
      - 8089:8089/udp
      - 2003:2003
      - 2003:2003/udp
      - 4242:4242
    volumes:
      - vmdata:/storage
    command:
      - "--storageDataPath=/storage"
      - "--graphiteListenAddr=:2003"
      - "--opentsdbListenAddr=:4242"
      - "--httpListenAddr=:8428"
      - "--influxListenAddr=:8089"
      - "--vmalert.proxyURL=http://vmalert:8880"
    restart: always
```
_Сохраняем и заходим в графану_

1) Заходим в Connections

2) В поиске вбиваем Prometheus
-  В правом верхнем углу будет кнопка "Add new data source"
-  В строке Connection вставляем htpp://victoriametrics:8428 и заменяем на имя Vika
-  Пролистываем вниз и нажимаем кнопку Save test

2)  Заходим в Dashboard
- Нажимаем в New
- Выбираем из предложенного New Dashboard
- Выбираем созданную

3) Возвращаемся в комнадную строку и прописываем:

- `curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'`

- `echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus`

4) Возвращаемся в графану
   
5) В открытом окне в строке переделываем на code, в строке Metrics browser вставляем **OILCOINT_metric1**

![image](https://github.com/user-attachments/assets/656b2cab-e1d9-439d-aa09-0b93ee93600e)

6) Обновляем
  
![image](https://github.com/user-attachments/assets/847ba0f6-3581-4a91-a04e-b1f94a0ec339)
