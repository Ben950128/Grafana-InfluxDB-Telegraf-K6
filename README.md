# Grafana-InfluxDB-Telegraf-K6

## Docker Swarm啟動grafana、telegraf、influxdb、k6  
#### 將要跑grafana跟influxdb的node設定一下，docker-compose.yml會吃下面兩行設定
#### docker node update --label-add influxdb=true <NODE-ID>
#### docker node update --label-add grafana=true <NODE-ID>

## 將grafana設定在nginx的reverse proxy  
#### 需先啟動一個docker container，將container內的/etc/grafana/grafana.ini複製到HOST，此container即可刪除  
#### 修正複製出來的grafana.ini，詳見https://grafana.com/tutorials/run-grafana-behind-a-proxy/  

## 先啟動Grafana、InfluxDB跟Telegraf  
#### docker stack deploy -c docker-compose.yml MONITORING  

## 再依據要跑的壓測啟動K6  
#### docker stack deploy -c k6-compose.yml k6-example  

## docker-compose.yml詳解
#### K6的network需設定為attachable，這樣後面啟的k6才能連上此network
#### telegraf要收集各node的資訊，因此用deploy mode:global
#### grafana有幾個volume只是是先將dashboard跟datasource餵進去，可以刪掉沒差
#### ./grafana.ini:/etc/grafana/grafana.ini是將修正好的ini檔取代原本container內的ini檔

## k6-compose.yml詳解  
#### entrypoint要先設定好跑哪個壓測檔案
#### 由於service會因為container死掉而繼續生出新的container，因此deploy設定restart policy為none讓service只要跑一次就好
