---
layout: post
title: The Evolution of Contexts in Apache Spark
subtitle: SparkContext, SQLContext, HiveContext and the Rise of SparkSession
categories: Tech
tags: [Spark, Bigdata]
excerpt_image: /assets/images/sparkcontext.png
---

#### Introduction:
The Apache Spark ecosystem has undergone significant transformations in the past years, with key components like SparkContext, SQLContext, and HiveContext adapting to meet the changing needs of big data processing. In this blog post, we'll look at the historical evolution of these components and their current role.

![Contexts of Spark](/assets/images/sparkcontext.png)
> Contexts of Spark


#### SparkContext: Foundation of Spark Applications
In Spark 1.0, `SparkContext`  a.k.a `sc` is the main entry point for the Spark application. Its major functions were to set up and initialize the Spark application, establish a connection with the Spark cluster, and coordinate the execution of tasks.

In Spark 2.0, while `SparkContext` retains its core functionality, the framework introduced a higher-level abstraction known as `SparkSession`.

In Spark 1.0, `SparkContext` uses `SparkConf` to configure the Spark setting as follows:

```python
from pyspark import SparkContext, SparkConf

# Create a SparkConf object with configuration settings
conf = SparkConf().setAppName("MyApp").setMaster("local")

# Create a SparkContext with the specified configuration
sc = SparkContext(conf=conf)
```

In Spark 2.0 and later versions, the creation of `SparkContext` was implicit.
```python
from pyspark.sql import SparkSession

# Create a SparkSession, which implicitly creates a SparkContext
spark = SparkSession.builder.appName("MyApp").getOrCreate()

# Accessing SparkContext from SparkSession
sc = spark.sparkContext
```

The `getOrCreate()` method ensures that a `SparkContext` is created if it does not exist.

#### SQLContext: Bridging the Structured Data Gap
In version 1.0, the emergence of `SQLContext` is a significant leap in structured data processing. It provides a high-level API to work with structured data using Spark SQL, allowing users to seamlessly integrate SQL queries into their Spark programs.

As the journey of Spark evolved, In 2.0 the `SQLContext` has been abstracted into `SparkSession` to provide a single entry point for the Spark ecosystem.

In Spark 1.0, `SQLContext` was created from `SparkContext`.
```python
from pyspark.sql import SQLContext
from pyspark import SparkContext, SparkConf

# Create a SparkConf object with configuration settings
conf = SparkConf().setAppName("MyApp").setMaster("local")

# Create a SparkContext with the specified configuration
sc = SparkContext(conf=conf)

# Create an SQLContext using the SparkContext
sqlContext = SQLContext(sc)
```

In Spark 2.0 and later versions:
```python
from pyspark.sql import SparkSession

# Create a SparkSession, which implicitly creates a SQLContext
spark = SparkSession.builder.appName("MyApp").getOrCreate()

# Accessing SQLContext from SparkSession
sqlContext = spark.sqlContext
```

#### HiveContext: Bridging Spark and Hive
In the Spark 1.0 era, `HiveContext` played a crucial role in bridging the gap between Spark and Apache Hive. It allowed Spark applications to leverage Hive's metadata and operate seamlessly on Hive tables.

`HiveContext` faced deprecation in Spark 2.0. `SparkSession` with Hive support was the recommended way to serve the community's intention to make SparkSession the single entry point.

Like `SQLContext`, `HiveContext` also is created using `SparkContext` in Spark 1.0
```python
from pyspark.sql import HiveContext
from pyspark import SparkContext, SparkConf

# Create a SparkConf object with configuration settings
conf = SparkConf().setAppName("MyApp").setMaster("local")

# Create a SparkContext with the specified configuration
sc = SparkContext(conf=conf)

# Create a HiveContext using the SparkContext
hiveContext = HiveContext(sc)
```  
In Spark 2.0:
```python
from pyspark.sql import SparkSession

# Create a SparkSession with Hive support
spark = SparkSession.builder.appName("MyApp").enableHiveSupport().getOrCreate()
```
The `enableHiveSupport()` method configures the `SparkSession` to work seamlessly with Hive, enabling you to execute Hive queries and work with Hive tables.

#### SparkSession: Unifying the Experience
The introduction of `SparkSession` not only combined the functionalities of `SparkContext`, `SQLContext`, and `HiveContext` but also provided a unified entry point for Spark applications. This consolidation simplified the user experience, making it easier to interact with Spark, Spark SQL, and Hive within a single interface. This evolution underscores the commitment of the Spark community to provide a cohesive and streamlined experience for big data processing.
