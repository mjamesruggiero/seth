# seth

<img src="http://33.media.tumblr.com/033b0bf60f062fe7cc440d669c9e380c/tumblr_mvchkl4PQT1qhbzd8o1_500.gif"/>

* Uses ScalaTest
* Uses Hadoop 2.5 (CDH5)

You can run SparkPi on local mode::

```
$ sbt "run local 1"
...
..
.
14/03/02 11:34:49 INFO SparkContext: Job finished: reduce at SparkPi.scala:37, took 0.610860249 s
pi: 3.11
14/03/02 11:34:50 INFO MapOutputTrackerMasterActor: MapOutputTrackerActor stopped!
14/03/02 11:34:50 INFO ConnectionManager: Selector thread was interrupted!
14/03/02 11:34:50 INFO ConnectionManager: ConnectionManager stopped
14/03/02 11:34:50 INFO MemoryStore: MemoryStore cleared
14/03/02 11:34:50 INFO BlockManager: BlockManager stopped
14/03/02 11:34:50 INFO BlockManagerMasterActor: Stopping BlockManagerMaster
14/03/02 11:34:50 INFO BlockManagerMaster: BlockManagerMaster stopped
14/03/02 11:34:50 INFO SparkContext: Successfully stopped SparkContext
14/03/02 11:34:50 INFO RemoteActorRefProvider$RemotingTerminator: Shutting down remote daemon.
[success] Total time: 7 s, completed 2014/03/02 11:34:50
14/03/02 11:34:50 INFO RemoteActorRefProvider$RemotingTerminator: Remote daemon shut down; proceeding with flushing remote transports.
```

# Run as a jar

```
$ sbt compile
$ sbt package
```

JAR will be `target/scala-2.10/basic-spark.jar`

Run SparkPi with spark-shell:

```
$ MASTER=local ADD_JARS=target/scala-2.10/basic-spark.jar SPARK_CLASSPATH=$SPARK_CLASSPATH:target/scala-2.10/basic-spark.jar spark-shell
```

Then import the library and run SparkPi:

```
scala> import com.example.SparkPi._
scala> val sp = new SparkPi(sc, 2)
scala> sp.exec()
...
..
.
res0: Double = 3.1376
```

# Run SparkPi on local mode

You can run SparkPi with spark-class command.

## Requirement

* sbt is installed
* Spark is installed on your computer.
* If you use CDH5, you have spark-class command in /usr/lib/spark/bin/spark-class.

## Procedure

First, you need compile source codes
and make JAR in the same way of running with spark-shell.
Then, we suppose that you have JAR as <your source root directory>/target/scala-2.10/basic-spark.jar.

Next, you can run SparkPi with spark-class command:

```
$ SPARK_CLASSPATH=$SPARK_CLASSPATH:target/scala-2.10/basic-spark.jar /usr/lib/spark/bin/spark-class com.example.SparkPi local
 14/03/02 11:51:01 INFO SparkContext: Job finished: reduce at SparkPi.scala:37, took 0.703761825 s
 pi: 3.1192
 14/03/02 11:51:02 INFO MapOutputTrackerMasterActor: MapOutputTrackerActor stopped!
 14/03/02 11:51:02 INFO ConnectionManager: Selector thread was interrupted!
 14/03/02 11:51:02 INFO ConnectionManager: ConnectionManager stopped
 14/03/02 11:51:02 INFO MemoryStore: MemoryStore cleared
 14/03/02 11:51:02 INFO BlockManager: BlockManager stopped
 14/03/02 11:51:02 INFO BlockManagerMasterActor: Stopping BlockManagerMaster
 14/03/02 11:51:02 INFO BlockManagerMaster: BlockManagerMaster stopped
 14/03/02 11:51:02 INFO RemoteActorRefProvider$RemotingTerminator: Shutting down remote daemon.
 14/03/02 11:51:02 INFO SparkContext: Successfully stopped SparkContext
 14/03/02 11:51:02 INFO RemoteActorRefProvider$RemotingTerminator: Remote daemon shut down; proceeding with flushing remote transports.
```

## How to run SparkPi on the Spark standalone cluster

You can run SparkPi on the Spark standalone cluster with spark-class command.

## Requirement

* sbt is installed
* Spark is installed on your computer.
  If you use CDH5, you have spark-class command
  in /usr/lib/spark/bin/spark-class.
* The standalone cluster of Spark is available from your computer.
  We assume that the url for the master is "spark://spark-01:7077".

## Procedure

First, you need to copy JAR to every server in the cluster.
In this tutorial, we assume that basic-spark.jar is located on /tmp/basic-spark.jar in every server,
and is readable for spark user.

Next, you can run SparkPi with spark-class command.::

```
 $ /usr/lib/spark/bin/spark-class org.apache.spark.deploy.Client launch spark://spark-01:7077 file:///tmp/basic-spark.jar com.example.SparkPi spark://spark-01:7077 10
 Sending launch command to spark://spark-01:7077
 Driver successfully submitted as driver-20140302163431-0000
 ... waiting before polling master for driver state
 ... polling master for driver state
 State of driver-20140302163431-0000 is RUNNING
 Driver running on spark-04:7078 (worker-20140228225630-spark-04-7078)
```

The launched driver program and application is found on Spark master's web frontend (e.g. http://spark-01:8080)
