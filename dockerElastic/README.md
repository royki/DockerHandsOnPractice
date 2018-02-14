##### _Run Elastic container in port 9200_

- `docker run -d -p 9200:9200 -p 9300:9300 -it -h elasticsearch --name elasticsearch elasticsearch`
- Check `curl http://localhost:9200/`


##### _Run Kibana container and connect with elastic logs_

- `docker run -d  -p 5601:5601 -h kibana --name kibana --link elasticsearch:elasticsearch kibana`
- Check`curl http://localhost:9200/_cat/indices`

##### _Run Logstash container with logstash config file and use it to setup Logstash_

- `docker run -h logstash --name logstash --link elasticsearch:elasticsearch -it --rm -v "$PWD":/config-dir logstash -f /config-dir/logstash.conf`
- `curl http://localhost:9200/_cat/indices`
- Run another container of logstash with another logconfig `docker run -d -p 9500:9500  -h logstash2 --name logstash2 --link elasticsearch:elasticsearch --rm -v "$PWD":/config-dir logstash -f /config-dir/logstash2.conf`

###### _Execute Logstash_
- `logstash -f config-dir/logstash.conf`


