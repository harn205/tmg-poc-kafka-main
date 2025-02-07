#Kafka poc template


#VM CLI
<pre><code>ls -lrt</code></pre>
<pre><code>ls -lrt</code></pre>
#Docker compose confluent</code></pre>
<pre><code>sudo docker-compose up -d</code></pre>
#show images 
<pre><code>sudo docker ps -a</code></pre>
#Web UI kafka external IP:port
<external IP>:9021

#Docker compose down</code></pre>
<pre><code>sudo docker-compose down</code></pre>
#delete folder
<pre><code>rm -rf foldername</code></pre>

#connector log
<pre><code>sudo docker logs connect</code></pre>




#Create source connector
<pre><code>curl -X GET  -H "Accept: application/json" -H "Content-Type: application/json" http://localhost:8083/connectors/ -d @mysql-source.json</code></pre>
#Create sink connector
<pre><code>curl -X GET  -H "Accept: application/json" -H "Content-Type: application/json" http://localhost:8083/connectors/ -d @mysql-sink-kafka.json</code></pre>


#Start ksqlDB
<pre><code>docker exce -it ksqldb http://localhost:8088</code></pre>
#Set offset 
ksql> <pre><code>SET 'auto.offset.reset'='earliest';</code></pre>
ksql> <pre><code>show topics;</code></pre>
ksql><pre><code>show connectors;</code></pre>
#CREATE STREAM <stram name >
ksql> <pre><code>CREATE STREAM ksqlstream WITH 
(KAFKA_TOPIC='kafka-class-db-001.demo.movies', VALUE_FORMAT = 'AVRO');</code></pre>
  
ksql> <pre><code>show streams;</code></pre>
  
#Create stream
  
ksql><pre><code> SELECT * FROM ksqlstream EMIT CHANGES LIMIT 10;</code></pre>
#Create stream trasform data
ksql> 
<pre><code>CREATE STREAM ksqlstream_processed AS 
SELECT 
after->movie_id AS movie_id,
after->title AS title,
2023 - CAST(after->release_year AS INT) AS num_year_release
FROM ksqlstream
WHERE op in ('c','u','r');</code></pre>
  
#where op:create uodate remove
ksql> <pre><code>show streams;</code></pre>
ksql> <pre><code>SELECT * FROM ksqlstream_processed EMIT CHANGES LIMIT 10;</code></pre>

ksql><pre><code>exit;</code></pre>

#Create sink connector
<pre><code>curl -X GET  -H "Accept: application/json" -H "Content-Type: application/json" http://localhost:8083/connectors/ -d @mysql-sink-ksqldb.json</code></pre>

