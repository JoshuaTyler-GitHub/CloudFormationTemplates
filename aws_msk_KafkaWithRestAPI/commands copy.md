arn:aws:kafka:us-east-1:466486113081:cluster/MSKCluster/efa6dcbc-b91b-482e-a331-7cbaf5e17924-1

./kafka-topics.sh --create --zookeeper z-2.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:2181,z-1.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:2181,z-3.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:2181 --replication-factor 3 --partitions 1 --topic KafkaDemoTopic

./kafka-rest-start /home/ec2-user/confluent-5.3.1/etc/kafka-rest/kafka-rest.properties
./kafka-rest-stop /home/ec2-user/confluent-5.3.1/etc/kafka-rest/kafka-rest.properties

./kafka-console-producer.sh --broker-list b-2.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:9092,b-1.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:9092,b-3.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:9092 --producer.config client.properties --topic KafkaDemoTopic

./kafka-console-consumer.sh --bootstrap-server b-2.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:9092,b-1.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:9092,b-3.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:9092 --topic KafkaDemoTopic --from-beginning 

https://d39aq79l1j.execute-api.us-east-1.amazonaws.com/dev/topics/KafkaDemoTopic