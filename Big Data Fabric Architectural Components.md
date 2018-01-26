## Big Data Fabric Architectural Components



### Data Flow

```mermaid
graph LR
source((fa:fa-lightbulb-o Sources));
source.->raw

style source fill:#ff704d,stroke:#333,stroke-width:4px;
raw["fa:fa-file-text RAW"]==>srs["fa:fa-list-ul Source Standardized"]
srs==>cfm["fa:fa-database Conformed"]
style raw fill:#f9f,stroke:#333,stroke-width:4px
style srs fill:#b3ffff,stroke:#333,stroke-width:4px
style cfm fill:#809fff,stroke:#333,stroke-width:4px
```

### Consumption:

```mermaid
graph TB
raw["fa:fa-file-text RAW"] 
srs["fa:fa-list-ul Source Standardized"]
 cfm["fa:fa-database Conformed"]
style raw fill:#f9f,stroke:#333,stroke-width:4px
style srs fill:#b3ffff,stroke:#333,stroke-width:4px
style cfm fill:#809fff,stroke:#333,stroke-width:4px
subgraph 
app("fa:fa-bar-chart-o AppZone")
style app fill:#ffff33,stroke:#333,stroke-width:4px
end
subgraph users
users(("fa:fa-users"))
end
users.->app
app.->raw
app.->srs
app.->cfm
```



### Technology Actors



#### Sources to Raw Zone

```mermaid
graph LR

style source fill:#ff704d,stroke:#333,stroke-width:4px;
style raw fill:#f9f,stroke:#333,stroke-width:4px;
subgraph Mechanism
RDBMS("fa:fa-database RDBMS: Oracle/SQL svr")
CDC("fa:fa-exchange Change Data Capture ")
FTP("fa:fa-file file transfer via SCP") 
RealTime("fa:fa-rocket RT Streaming via Kafka")
RDBMS.->raw
FTP.->raw
RealTime.->raw
CDC.->raw
end
subgraph CDC
Kafka("Kafka Topics")
Cobol("Cobol Serde")
 Kafka
 Cobol
end

subgraph Streaming
KConsume("Kafka Consumer")
KConnect("Kafka Connect")
end
subgraph RDBMS
Sqoop("Sqoop")
end

source((fa:fa-lightbulb-o Sources))
raw["fa:fa-file-text RAW"]
source-->FTP
source-->CDC
source-->RealTime
source-->RDBMS

```

#### Raw to Source Standardized

```mermaid
graph LR
raw["fa:fa-file-text RAW"]
srs["fa:fa-list-ul Source Standardized"]
style raw fill:#f9f,stroke:#333,stroke-width:4px;
style srs fill:#b3ffff,stroke:#333,stroke-width:4px
subgraph Components
Hive("Hive Query")
Spark("Spark Jobs") 
StreamSets("StreamSets")
Hive.->srs
Spark.->srs
StreamSets.->srs
raw==>Hive
raw==>Spark
raw==>StreamSets
end
```

#### Source Standardized to Conformed

```mermaid
graph LR
srs["fa:fa-list-ul Source Standardized"]
 cfm["fa:fa-database Conformed"]
srs.->Hive
srs.->Spark
style srs fill:#b3ffff,stroke:#333,stroke-width:4px
style cfm fill:#809fff,stroke:#333,stroke-width:4px
subgraph Approaches
Hive("Hive Query")
Spark("Spark Jobs")
end
subgraph Conformed
avro("fa:fa-file-code-o AVRO Files")
cfm.-> avro
Hive-->cfm
Spark-->cfm
end

```

