# SF-Crime-Spark-Kafka-Integration

# Streaming SF Crime Data Into Kafka and providing a statistical analyses of the data using Apache Spark Structured Streaming.

# In this Project You will draw on the skills and knowledge you've learned to create a Kafka server to produce data, and ingest data through Spark Structured Streaming.

# Development Environment
  Spark 2.4.3
  Scala 2.11.x
  Java 1.8.x
  Kafka build with Scala 2.11.x
  Python 3.6.x or 3.7.x

# Environment Setup For Macs or Linux:
  Download Spark from https://spark.apache.org/downloads.html. Choose "Prebuilt for Apache Hadoop 2.7 and later."
  Unpack Spark in one of your folders (I usually put all my dev requirements in /home/users/user/dev).
  Download binary for Kafka from this location https://kafka.apache.org/downloads, with Scala 2.11, version 2.3.0. Unzip in your local directory where you unzipped your Spark       binary as well. Exploring the Kafka folder, you’ll see the scripts to execute in bin folders, and config files under config folder. You’ll need to modify zookeeper.properties     and server.properties.
  Download Scala from the official site, or for Mac users, you can also use brew install scala, but make sure you download version 2.11.x.
  Run below to verify correct versions:
  java -version
  scala -version
  Make sure your ~/.bash_profile looks like below (might be different depending on your directory):
  export SPARK_HOME=/Users/dev/spark-2.4.3-bin-hadoop2.7
  export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home
  export SCALA_HOME=/usr/local/scala/
  export PATH=$JAVA_HOME/bin:$SPARK_HOME/bin:$SCALA_HOME/bin:$PATH
  
# For Windows:
  Please follow the directions found in this helpful StackOverflow post: https://stackoverflow.com/questions/25481325/how-to-set-up-spark-on-windows
  
# Beginning the Project
 This project requires creating topics, starting Zookeeper and Kafka servers, and your Kafka bootstrap server. You’ll need to choose a port number (e.g., 9092, 9093..) for your   Kafka topic, and come up with a Kafka topic name and modify the zookeeper.properties and server.properties appropriately.

# Local Environment
 Install requirements using ./start.sh if you use conda for Python. If you use pip rather than conda, then use pip install -r requirements.txt.

 Use the commands below to start the Zookeeper and Kafka servers. You can find the bin and config folder in the Kafka binary that you have downloaded and unzipped.

   bin/zookeeper-server-start.sh config/zookeeper.properties
   bin/kafka-server-start.sh config/server.properties
 
   You can start the bootstrap server using this Python command: python kafka_server.py.

# Step 1
 The first step is to build a simple Kafka server and Complete the code for the server in producer_server.py and kafka_server.py.
 
# Local Environment
 To see if you correctly implemented the server, use the command bin/kafka-console-consumer --bootstrap-server localhost:<your-port-number> --topic <your-topic-name> --from-       beginning to see your output.
  
# Step 2
Apache Spark already has an integration with Kafka brokers, so we would not normally need a separate Kafka consumer. However, create one anyway. Why? it will be good to create the consumer to demonstrate your understanding of creating a complete Kafka Module (producer and consumer) from scratch. In production, you might have to create a dummy producer or consumer to just test out your theory and this will be great practice for that.

# Implement all the TODO items in data_stream.py. To do some Data Analysis You may need to explore the dataset beforehand using a Jupyter Notebook which I Included.

# Do a spark-submit using this command: 

spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.4 --master local[*] data_stream.py.
  Check the progress reporter after executing a Spark job.
  Check the Spark Streaming UI as the streaming continues.
  
# Step 3

Write the answers to these questions in the README.md doc of your GitHub repo:

How did changing values on the SparkSession property parameters affect the throughput and latency of the data?
 ---> SparkSession property parameters were observed by measuring the processedRowspersecond parameter in the Progress Report. The higher number we set in spark.conf.set, the         higher the processing rows per second increases which means High thorugh put and low latecy of the data.
 
What were the 2-3 most efficient SparkSession property key/value pairs? Through testing multiple variations on values, how can you tell these were the most optimal?

 ---> The Infrasturcture and the Data needs to be Analysed first before we decide on making configuration changes on SparkSession Properties. If we are working on Distributed         Computing Architecture then it will depend on the number of nodes we have to process the data. If we have quad-core Processors to support the                                     spark.default.parallelism we could achieve more rows being processed in less amount of time. spark.streaming.kafka.maxRatePartition is also useful in making the streaming       process more stable and efficient. Large Number of Unprocessed messages will be prevented by setting the maximum rate for each partition. Lastly we can decider the number       of partitions for the Data set is to divide the dataset size / partition size and providing the desire count in spark.sql.shuffle.partitions. 
