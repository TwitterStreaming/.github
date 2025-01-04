 # Twitter Stream Processing Pipeline

This project focuses on designing and implementing a complete data pipeline to handle streaming tweets, process the data, and visualize the results in a user-friendly web application. The system integrates several components to ensure real-time data ingestion, processing, and visualization.

## Documentation quick links

- [Pipeline Components](#pipeline-components)
    - [Tweet Stream Ingestion](#1-tweet-stream-ingestion)
    - [Tweet Processing Pipeline](#2-tweet-processing-pipeline)
    - [Storage](#3-storage)
    - [Web Application](#4-web-application)
- [Installation](#installation)
    - [Linux/macOS](#linuxmacos-unix)
    - [Windows](#windows)
- [Kafka and Zookeeper](#kafka-and-zookeeper)
    - [Linux/macOS](#linuxmacos-unix-1)
    - [Windows](#windows-1)

## Pipeline Components

### 1. **Tweet Stream Ingestion**

- A producer program reads data from the `tweets.json` file and streams it to an Apache Kafka topic named `raw-tweets`.

### 2. **Tweet Processing Pipeline**

The pipeline includes three consumers to handle data processing:

- **Consumer 1**: Written in Scala with Spark, it extracts key fields (e.g., text, creation time, geo-coordinates, hashtags) from tweets in the `raw-tweets` Kafka topic and writes the processed data to the `transform-tweets` topic.
- **Consumer 2**: Performs sentiment analysis using Spark NLP or TextBlob on tweets from the `transform-tweets` topic, adding sentiment data and forwarding results to the `sentiment-tweets` topic.
- **Consumer 3**: A Python consumer writes tweets from the `sentiment-tweets` topic into an Elasticsearch database for querying and visualization.

### 3. **Storage**

- Stores processed tweets and metadata in **Elasticsearch** database.
- Implements an efficient schema design to support fast querying for tweet annotations and visualization.

### 4. **Web Application**

The web application provides an interactive interface with the following features:

- **Keyword Search**: Allows users to enter a keyword to query tweets.
- **Geospatial Visualization**: Renders tweets containing the keyword on a map based on their geo-coordinates (longitude/latitude).
- **Trend Diagram**: Displays a temporal distribution of tweets over time with hourly and daily aggregation levels.
- **Sentiment Analysis Gauge**: Visualizes the average sentiment score of tweets over a specified period.

---

This pipeline demonstrates the integration of modern data engineering tools and techniques to build a robust, scalable, and user-friendly system for real-time data processing and visualization.

## Installation

### Linux/macOS (Unix)

As for each [Consumer, Producer](https://github.com/TwitterStreaming/ConsumerAndProducer/tree/dev) and [Backend-Client](https://github.com/TwitterStreaming/Backend-Client/tree/dev)

```bash
# cd into the directory that you want to run
make init
source venv/bin/activate
make install
make run
```

**NOTE**: For Elasticsearch Consumer and Backend-Client, don't forget to change the password to your Elasticsearch password

Open Consumer as project in IntelliJ

As for the [web application](https://github.com/TwitterStreaming/ConsumerAndProducer/tree/dev)

```bash
npm i
npm start
```

### Windows

As for each Consumer, Producer and Backend-Client

```powershell
Remove-Item -Recurse -Force venv # rmdir venv for cmd
py -m venv venv # py, python or python3
.\venv\Scripts\activate
```

To install dependencies and run for each one of them

```powershell
# PythonConsumer
pip install nltk pandas scikit-learn textblob confluent-kafka
py consumer.py # py, python or python3

# ElasticSearchConsumer
pip install confluent-kafka elasticsearch python-dotenv
py elasticSeachConsumer.py # py, python or python3

# Producer
pip install confluent-kafka
py producer.py # py, python or python3

# Backend-Client
pip install django django-cors-headers elasticsearch python-dotenv
py manage.py runserver # py, python or python3
```

**NOTE**: For Elasticsearch Consumer and Backend-Client, don't forget to change the password to your Elasticsearch password

Open Consumer as project in IntelliJ

As for the [web application](https://github.com/TwitterStreaming/ConsumerAndProducer/tree/dev)

```bash
npm i
npm start
```

## Kafka and Zookeeper

### Linux/macOS (Unix)

To install Kafka and Zookeeper

```bash
curl "https://dlcdn.apache.org/kafka/3.8.1/kafka_2.12-3.8.1.tgz" --output ~/kafka.tgz
tar -xvzf ~/kafka.tgz
rm ~/kafka.tgz
mv kafka_2.12-3.8.1 ~/kafka
```

Then to run the servers

```bash
~/kafka/bin/zookeeper-server-start.sh ~/kafka/config/zookeeper.properties
~/kafka/bin/kafka-server-start.sh ~/kafka/config/server.properties
```

### Windows

In Windows, it’s a bit different as you have to download it via this [link](https://dlcdn.apache.org/kafka/3.8.1/kafka_2.12-3.8.1.tgz), then extract it and place the folder in C:\ and name it `kafka`, then run the servers via

```powershell
C:\kafka\bin\windows\zookeeper-server-start.bat C:\kafka\config\zookeeper.properties
C:\kafka\bin\windows\kafka-server-start.bat C:\kafka\config\server.properties
```

<img width="6456" alt="Twitter Stream Processing Pipeline figure (1)" src="https://github.com/user-attachments/assets/045a1355-faff-4ad5-af62-73298175f046" />

For a higher quality, click this [link](https://drive.google.com/file/d/1aqYEskg_q7dLKWS8S1zGCcB5T_gA6Acj/view?usp=sharing)
