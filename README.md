# Grafana-InfluxDB-Telegraf-K6

將grafana設定在nginx的reveerse proxy  
需先啟動一個docker container，將container內的/etc/grafana/grafana.ini複製到HOST，此container即可刪除  
修正複製出來的grafana.ini，詳見https://grafana.com/tutorials/run-grafana-behind-a-proxy/  

先啟動Grafana、InfluxDB跟Telegraf  
docker stack deploy -c docker-compose.yml MONITORING  

再依據要跑的壓測啟動K6  
docker stack deploy -c k6-compose.yml k6-example  
