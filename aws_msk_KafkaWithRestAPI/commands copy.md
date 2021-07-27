# All variables quick references
ARN: <Cluster_ARN>
BOOTSTRAPBROKER: <BootstrapBrokerString(TLS)>
JVM RUNTIME: <JVM_Runtime>
REGION: <Region>
TOPIC NAME: <Topic_Name>
ZOOKEEPER: <ZooKeeperConnectionString>


# 1) Get the <Cluster_ARN> & <Region> from CloudFormation Output
ARN: <Cluster_ARN>
REGION: <Region> (replace with e.g. us-east-1) 

# 2) Get the <ZooKeeperConnectionString> and other information about your cluster
$ aws kafka describe-cluster 
    --region <Region>
    --cluster-arn <Cluster_ARN>

ZOOKEEPER: <ZooKeeperConnectionString>

# 3) Get the <BootstrapBrokerString(TLS)> by entering the following code provide your region & cluster ARN
BOOTSTRAPBROKER: <BootstrapBrokerString(TLS)> (may or may not end with TLS depending on security rules)

$ aws kafka get-bootstrap-brokers
    --region <Region>
    --cluster-arn <Cluster_ARN>

# 4) Get your Java JVM JRE security cacerts & copy them to kafka truststore
JVM RUNTIME: <JVM_Runtime> (e.g. java-1.8.0-openjdk-1.8.0.282.b08-1.amzn2.0.1.x86_64)

cp /usr/lib/jvm/<JVM_Runtime>/jre/lib/security/cacerts /tmp/kafka.client.truststore.jks

# 5) Setup your client.properties folder in the kafka/bin
sudo touch client.properties
sudo chmod client.properties 777
vim client.properties

=======================
security.protocol=SSL
ssl.truststore.location=/tmp/kafka.client.truststore.jks
=======================

# 6) Create a Kafka Topic by providing the <ZooKeeperConnectionString> from step 2 (you should receive a confirmation message):
TOPIC NAME: <Topic_Name>

$ ./kafka-topics.sh --create --zookeeper <ZooKeeperConnectionString> --replication-factor 3 --partitions 1 --topic <Topic_Name>

# 7) Test Kafka by:
# 7.1) reading (consuming) messages with:
./kafka-console-consumer.sh --bootstrap-server <BootstrapBrokerString(TLS)> --topic <Topic_Name> --from-beginning 

# 7.2) writing (producing) messages with:
./kafka-console-producer.sh --broker-list <BootstrapBrokerString(TLS)> --producer.config client.properties --topic <Topic_Name>


