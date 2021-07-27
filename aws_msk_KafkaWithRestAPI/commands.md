# 1) SSH into the EC2 instance created

# 2) Get the <ZooKeeperConnectionString> and other information about your cluster by entering the following code
$ aws kafka describe-cluster 
    --region <Replace_With_us-east-1_or_us-west-2> 
    --cluster-arn <Replace_With_Your_cluster-arn> 

# 3) Get the <BootstrapBrokerString> by entering the following code provide your region & cluster ARN
$ aws kafka get-bootstrap-brokers
    --region <Replace_With_us-east-1_or_us-west-2>
    --cluster-arn <Replace_With_us-east-1_or_us-west-2>

# 4) Go to the bin folder (kafka/kafka_2.12-2.2.1/bin/) of the Apache Kafka installation on the client machine.

# 5) Create a Kafka Topic by providing the <ZooKeeperConnectionString> from step 3 (you should receive a confirmation message):
$ ./kafka-topics.sh
    --create
    --zookeeper <Replace_With_Your_ZookeeperConnectString>
    --replication-factor 3
    --partitions 1
    --topic <Replace_With_Your_Desired_Topic_Name_String>

# 6) To connect the Kafka REST server to the Amazon MSK cluster, modify kafka-rest.properties in the directory (/home/ec2-user/confluent-5.3.1/etc/kafka-rest/) to point to your Amazon MSK’s <ZooKeeperConnectionString> and <BootstrapBrokerString> information.

# 7) Generate the server and client SSL Certificates. For more information, see Creating SLL Keys and Certificates on the Confluent website. Add the necessary property configurations to the kafka-rest.properties:
    listeners=http://0.0.0.0:8082,https://0.0.0.0:8085
    ssl.truststore.location=<Replace_With_Your_tuststore.jks>
    ssl.truststore.password=<Replace_With_Your_tuststorepassword>
    ssl.keystore.location=<Replace_With_Your_keystore.jks>
    ssl.keystore.password=<Replace_With_Your_keystorepassword>
    ssl.key.password=<Replace_With_Your_sslkeypassword>

# 8) Configure API Gateway
    9.1) On the API Gateway console, choose Create API.
    9.2) For API type, choose REST API.
    9.3) Choose Build.
    9.4) Choose New API.
    9.5) For API Name, enter a name (for example, amazonmsk-restapi).
    9.6) As an optional step, for Description, enter a brief description.
    9.7) Choose Create API.The next step is to create a child resource.
    9.8) Under Resources, choose a parent resource item.
    9.9) Under Actions, choose Create Resource.The New Child Resource pane opens.
    9.10) Select Configure as proxy resource.
    9.11) For Resource Name, enter proxy.
    9.12) For Resource Path, enter /{proxy+}.
    9.13) Select Enable API Gateway CORS.
    9.14) Choose Create Resource.
    9.15) For Integration type, select HTTP Proxy.
    9.16) For Endpoint URL, enter an HTTP backend resource URL (your Kafka Clien Amazont EC2 instance PublicDNS; for example, http://KafkaClientEC2InstancePublicDNS:8082/{proxy} or https://KafkaClientEC2InstancePublicDNS:8085/{proxy}).
    9.17) Use the default settings for the remaining fields.
    9.18) Choose Save.
    9.19) For SSL, for Endpoint URL, use the HTTPS endpoint.
    9.20) Choose the API you just created.
    9.21) Under Actions, choose Deploy API.
    9.22) For Deployment stage, choose New Stage.
    9.23) For Stage name, enter the stage name (for example, dev, test, or prod).
    9.24) Choose Deploy.
    9.25) Record the Invoke URL after you have deployed the API.

# 9) Test the end-to-end processes by producing and consuming messages to Amazon MSK. Complete the following steps:
    10.1) Go to the confluent-5.3.1/bin directory and start the kafka-rest service. See the following code:
            $ ./kafka-rest-start /home/ec2-user/confluent-5.3.1/etc/kafka-rest/kafka-rest.properties
        If the service already started, you can stop it with the following code:
            $ ./kafka-rest-stop /home/ec2-user/confluent-5.3.1/etc/kafka-rest/kafka-rest.properties
    10.2) Open another terminal and navigate to the kafka/kafka_2.12-2.2.1/bin directory and start the Kafka console consumer. See the following code:
            $ ./kafka-console-consumer.sh
                --bootstrap-server <Replace_With_Your_BootstrapserversConnectString>
                --topic <Replace_With_Your_Desired_Topic_Name_String>
                --from-beginning 
        You can now produce messages using Postman. Postman is an HTTP client for testing web services.
        Be sure to open TCP ports on the Kafka client security group for the system you are running Postman on.
    10.3) Postman settings:
        The URL is your API Gateway Invoke URL /topics/<Replace_With_Your_Desired_Topic_Name_String>
        Under Headers, choose the key Content-Type with value application/vnd.kafka.json.v2+json
        Under Body, select raw.
        Choose JSON. Test post with the following JSON Object:
        {"records":[{"value":{"deviceid": "AppleWatch4","heartrate": "72","timestamp":"2019-10-07 12:46:13"}}]} 