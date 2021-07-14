# 1) SSH into the EC2 instance created

# 2) Configure your AWS Credentials
$ aws configure

# 3) Get the <ZooKeeperConnectionString> and other information about your cluster by entering the following code
$ aws kafka describe-cluster 
    --region <Replace_With_us-east-1_or_us-west-2> 
    --cluster-arn <Replace_With_Your_cluster-arn>  

aws kafka describe-cluster --region us-east-1 --cluster-arn arn:aws:kafka:us-east-1:466486113081:cluster/MSKCluster/efa6dcbc-b91b-482e-a331-7cbaf5e17924-1

# 4) Get the BootstrapBrokerString by entering the following code provide your region & cluster ARN
$ aws kafka get-bootstrap-brokers
    --region <Replace_With_us-east-1_or_us-west-2>
    --cluster-arn <Replace_With_us-east-1_or_us-west-2>

aws kafka get-bootstrap-brokers --region us-east-1 --cluster-arn arn:aws:kafka:us-east-1:466486113081:cluster/MSKCluster/efa6dcbc-b91b-482e-a331-7cbaf5e17924-1

# 5) Go to the bin folder (kafka/kafka_2.12-2.2.1/bin/) of the Apache Kafka installation on the client machine.

# 6) Create a Kafka Topic by providing the <ZooKeeperConnectionString> from step 3:
$ ./kafka-topics.sh
    --create
    --zookeeper <Replace_With_Your_ZookeeperConnectString>
    --replication-factor 3
    --partitions 1
    --topic <Replace_With_Your_Desired_Topic_Name_String>

./kafka-topics.sh --create --zookeeper z-2.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:2181,z-1.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:2181,z-3.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:2181 --replication-factor 3 --partitions 1 --topic KafkaDemoTopic

./kafka-console-consumer.sh --bootstrap-server b-2.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:9092,b-1.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:9092,b-3.mskcluster.lkezqu.c1.kafka.us-east-1.amazonaws.com:9092 --topic KafkaDemoTopic --from-beginning 

https://d39aq79l1j.execute-api.us-east-1.amazonaws.com/dev/topics/KafkaDemoTopic