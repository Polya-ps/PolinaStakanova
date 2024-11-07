# Polina Stakanova

Перед началом установки, нужно установить Linux Oracle на VirtualBox, для этого выполним ряд действий:

Создаем виртуальную машину. В "образ ISO" на диске Student, в папке К-ИСП-49,выбираем папку Давыдов АА и там лежит документ.

Иметь образ Linux, добаывить побольше ядер и опертивной паямяти.

**Далее переходим к установке docker с использованием grafana, вводим следующий набор команд:**

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

**Установка и настройка Docker**
1) `sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
`
![image](https://github.com/user-attachments/assets/f8041f63-d355-46f9-8208-6e40a871597d)

2)` sudo yum install docker-ce docker-ce`
![image](https://github.com/user-attachments/assets/a51b6314-c1cf-4dc2-8a11-fa2f73299701)
![image](https://github.com/user-attachments/assets/c8756496-b0e2-47a1-b168-cd00747e5bde)

3) `sudo systemctl enable docker --now`

4) `sudo docker compose up -d`

![image](https://github.com/user-attachments/assets/755275c4-d0fa-49b6-a939-09c776d50eec)

   Заходим на сайт Local host:3000
Через admin, admin

На сайте заходим в Dashboards, следом в Create Dashboard, следом Add visualization, следом Configure a new date source.

Из предложенных выбираем Prometheus после этого прописываем в строке Connection http://prometheus:9090

В  строке Authentication выбираем из предложенных Basic authentication, следом заходим через admin, admin

Дальше прожимаем save+test

Дальше импортируем 

Find and import dashboards for common applications at grafana.com/dashboards: 1860  //ждем кнопку Load
Select Prometheus
ждем кнопку "Import"

Возвращаемся в командную строку и прописываем: 

cd grafana_starck_for_docker/

sudo vi docker-compose.yaml

В открывшейся строке мы удаляем предложенное и втсавляем из папки config код

После в комнадной строке ниже пишем :wq!

Прописываем:
1. docker compose up -d
2. Заходим в графану
3. Заходим в Connections
4. В поиске вбиваем Prometheus
5. В правом верхнем углу будет кнопка "Add new data source"
6. В строке Connection вставляем htpp://victoriametrics:8428
7. Пролистываем вниз и нажимаем кнопку Save test
8. Заходим в Dashboard
9. Нажимаем в New
10. Выбираем из предложенного New Dashboard
11. Выбираем созданную
12. Возвращаемся в комнадную строку и прописываем:
13. curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'
14. echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus
15. Возвращаемся ы графану
16. В открытом окне в строке переделываем на code, в строке Metrics browser вставляем OILCOINT_metric1
      ![image](https://github.com/user-attachments/assets/656b2cab-e1d9-439d-aa09-0b93ee93600e)
17. Обновляем
![image](https://github.com/user-attachments/assets/847ba0f6-3581-4a91-a04e-b1f94a0ec339)
