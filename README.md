# REFERENCES
    GET Started - https://kafka.apache.org/quickstart   
    Kafka Download - https://www.apache.org/dyn/closer.cgi?path=/kafka/3.1.0/kafka_2.13-3.1.0.tgz
    AWS managed Kafka - https://aws.amazon.com/msk/
    Spring boot kafka - https://spring.io/projects/spring-kafka

# BOOTSTRAP PROJECT FOR KAFKA      
        download from http link,
        cd donwloads
        % tar -xzf kafka_2.13-3.1.0.tgz
        % rm -rf kafka_2.13-3.1.0.tgz
        %  cd kafka_2.13-3.1.0
        //Start Zookeper
        % bin/zookeeper-server-start.sh config/zookeeper.properties
        //Start Broker
        % cd Downloads
        % cd kafka_2.13-3.1.0
        % bin/kafka-server-start.sh config/server.properties
        //We should see message like
        from now on will use broker 192.168.0.193:9092

# BOOTSTRAP Project for kafka
        Spring Initialiser-->Name - Kafka-example Dependencies -  select spring web and kafka dependency
**KafkaTopicConfig**
        
        will be responsible for Topic creations.
**Kafka Producer Config** 
        
        added producer config and commandline runner bean
        when we run the application, we should see below example log 
        [ad | producer-1] org.apache.kafka.clients.Metadata        : [Producer clientId=producer-1] Cluster ID: -FI6NEOFQXegS256TAEiSg
        //TODO: Replace it with Restful API
      
**kafka Consumer - Reading Events**
       
         % cd Downloads
        % cd kafka_2.13-3.1.0
        % bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092

        kafkaCOnsuerConfig.java contains the consumer config 

**kafka Listener**

    Listener created in kafkaListeners.java
    
    we should see something below in the logs
    2022-05-22 20:09:02.135  INFO 88015 --- [ntainer#0-0-C-1] o.a.k.c.c.internals.ConsumerCoordinator  : [Consumer clientId=consumer-groupID-1, groupId=groupID] Setting offset for partition Abhilashgd-0 to the committed offset FetchPosition{offset=200, offsetEpoch=Optional.empty, currentLeader=LeaderAndEpoch{leader=Optional[192.168.0.193:9092 (id: 0 rack: null)], epoch=0}}
    2022-05-22 20:09:02.135  INFO 88015 --- [ntainer#0-0-C-1] o.s.k.l.KafkaMessageListenerContainer    : groupID: partitions assigned: [AAbhilashgd-0]
    Listener received: hello Kafka 0 :)
    Listener received: hello Kafka 1 :)
    Listener received: hello Kafka 2 :)
    Listener received: hello Kafka 3 :)

**kafka Listener - RestFul API and Kafk Integration**

    Record: MessageRequest
    API: MessageController
    Topic: Abhilashgd
    //POSTMAN TESTING
    POST: http://localhost:8088/api/v1/messages
    JSON BODY RAW: 
        {
         "message": "API request for Kafka"
        }
    
    in the application log we should see 
    2022-05-22 20:49:11.930  INFO 91366 --- [ad | producer-1] org.apache.kafka.clients.Metadata        : [Producer clientId=producer-1] Resetting the last seen epoch of partition Abhilashgd-0 to 0 since the associated topicId changed from null to 1aLdUK-4RJ6L9aMt2mnJig
    Listener received: API request for Kafka :)

**Custom Objects**

    Instead of sending just a string message, we can send custom objects in kafka templates
    Record: Message
    API: MessageController
    Message message = new Message(request.message(), LocalDateTime.now());
    Producer: value will come from JSONSerialiser instead of Spring Serializer
    Consumer:
    new StringDeserializer(),
    new JsonDeserializer<>()
    Message object
    Application:
    Kafka template configured with Message Object

    In the logs we should see
    2022-05-22 21:18:19.857  INFO 5987 --- [ntainer#0-0-C-1] o.a.k.c.c.internals.ConsumerCoordinator  : [Consumer clientId=consumer-groupID-1, groupId=groupID] Setting offset for partition Abhilashgds-0 to the committed offset FetchPosition{offset=401, offsetEpoch=Optional.empty, currentLeader=LeaderAndEpoch{leader=Optional[192.168.0.193:9092 (id: 0 rack: null)], epoch=0}}
    2022-05-22 21:18:19.857  INFO 5987 --- [ntainer#0-0-C-1] o.s.k.l.KafkaMessageListenerContainer    : groupID: partitions assigned: [Abhilashgd-0]
    Listener received: {"message":"hello Abhilashgd Kafka 0","createdAt":[2022,5,22,21,18,19,751548000]} :)
    Listener received: {"message":"hello Abhilashgd Kafka 1","createdAt":[2022,5,22,21,18,19,815280000]} :)
    Listener received: {"message":"hello Abhilashgd Kafka 2","createdAt":[2022,5,22,21,18,19,815508000]} :)
    Listener received: {"message":"hello Abhilashgd Kafka 3","createdAt":[2022,5,22,21,18,19,815578000]} :)

    //POSTMAN TESTING
    POST: http://localhost:8088/api/v1/messages
    JSON BODY RAW: 
        {
         "message": "Hello AAbhilashgd Object"
        }
    Log:
    2022-05-22 21:23:27.390  INFO 5987 --- [nio-8088-exec-2] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
    2022-05-22 21:23:27.391  INFO 5987 --- [nio-8088-exec-2] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
    2022-05-22 21:23:27.396  INFO 5987 --- [nio-8088-exec-2] o.s.web.servlet.DispatcherServlet        : Completed initialization in 5 ms
    Listener received: {"message":"Hello Abhilashgd Object","createdAt":[2022,5,22,21,23,27,439588000]} :)

**Trusted Packages**
    
    Listener:  containerFactory = "messageFactory"
    KafkaConsumerConfig - 
    jsonDeserializer.addTrustedPackages("com.abhilashgd");
    or
    jsonDeserializer.addTrustedPackages("*");
    messageFactory Bean
    LOGS:
    2022-05-22 21:41:56.786  INFO 12299 --- [ntainer#0-0-C-1] o.s.k.l.KafkaMessageListenerContainer    : groupID: partitions assigned: [Abhilashgds-0]
    Listener received: Message[message=hello Abhilashgd Kafka 32, createdAt=2022-05-22T21:26:52.617954] :)
    Listener received: Message[message=hello Abhilashgd Kafka 33, createdAt=2022-05-22T21:26:52.618009] :)

**AWS Managed Kafka & Spring boot kafka**

    References : 
    https://aws.amazon.com/msk/
    https://spring.io/projects/spring-kafka
    
**KAFKA handling millions of Events**
       
    In the KafkaApplication.java, replace for loop counter with 10_000_000 in place of 100 and run the application.
    
