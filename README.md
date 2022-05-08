# KAFKA-DOC1
Fundamentals
       
        - Kafka Documentation: https://kafka.apache.org/documentation/
        Introduction - 
        - Distributed Streaming Platform
        - Real time Event driven Application (example uber driver location update)
        - Fault tolerant
        - Scalable
        - Kafka runs a cluster,
        - Kafka connect source for data integration
        
        Producers send events of streams to kafka brokers and consumers recieve those events.it works through topics.
        Brokers running our Kafka process

        //Difference between Kafka and RABBITMQ
        in kafka, messages sent to the topic dont dissapear, they stay till retention period.
        in a traditional message queue, messages are gone as soon as they are consumed.
        
        Data Processing and transformation library
        - Data Transformation
        - Enrich Data
        - Filtering, Grouping, Aggregating, Joining and more

# Kafka Streams JAVA API

        - performs all data processing and puts it back to the topic so that consumers can consume
        
        //Kafka Topics - Producer - log - consumer
        Collection of Events,
        Replicated and partitioned,
        Durable - hours, days, years, forever
        Big or small

# Commands 

        GET Started - https://kafka.apache.org/quickstart
        Kafka Download - https://www.apache.org/dyn/closer.cgi?path=/kafka/3.1.0/kafka_2.13-3.1.0.tgz
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
        
        //BOOTSTRAP Project for kafka
        Spring Initialiser-->Name - Kafka-example Dependencies -  select spring web and kafka dependency
