# Replayer-Starter
Starter code to replay data in a file as a stream

## Running code Locally
Install maven and git.
Following are the steps to run the code on your local system:
 ```
 git clone https://github.com/prajay/Replayer-Starter.git
 cd Replayer-Starter/storm
 mvn clean compile package
 ``` 
You can then test the topology with the following code:
```
java -jar target/iot-bm-storm-0.1-jar-with-dependencies.jar <L|C> <Topology Name> </path/to/input/file> <Log ID> <Scale Factor> <path/to/log/dir> dummy test
```
where each option is as follows:
> L|C - Stands for Local/Cluster. Use L when running the code on local system, and C when running on the cluster

> Topology Name - Give a name to the topology. (The log file names will have the topology name added) 

> /path/to/input/file - The path to the input file. The file content should be a CSV as described [here](#format-of-input-file)

> Log ID - Unique ID that is used to create the log files (log file name uses this). Ensure this is changed each time you run the code so that a separate log file is created

> Scale factor - A floating point value that is the inverse of the speed up that you require. If you want 100X speedup, then set this value to (1/100) = 0.01. For 10X set this to 0.1.

> /path/to/log/dir - Path to directory where logs need to be stored

> dummy test - Leave these arguments as is.

E.G:
```
java -jar target/iot-bm-storm-0.1-jar-with-dependencies.jar L IdentityTopology /home/dreamlab2/newtestfile JSON-210 0.001 /home/dreamlab2/Desktop/log dummy test
```
## Format of Input File
The Input file to this code must be a CSV file with the following format:
> Epoch Time, JSON Data
where the lines are sorted by Epoch Time.

A sample file is located [here](storm/SYS_sample_data_senml.csv)

## Running code on the cluster
Clone the repository onto the cluster. 
You need to make the following changes to [storm/pom.xml](storm/pom.xml) before building and running the jar on the cluser:
```
diff --git a/storm/pom.xml b/storm/pom.xml
index 92373b0..105826a 100644
--- a/storm/pom.xml
+++ b/storm/pom.xml
@@ -33,7 +33,7 @@
                                </descriptorRefs>
                                <archive>
                                <manifest>
-                               <mainClass>in.dream_lab.bm.stream_iot.storm.topo.apps.SampleTopology</mainClass>
+                               <mainClass></mainClass>
                                </manifest>
                                </archive>
                                </configuration>
@@ -55,7 +55,7 @@
                        <groupId>org.apache.storm</groupId>
                        <artifactId>storm-core</artifactId>
                        <version>1.0.1</version>
-                       <!--<scope>provided</scope>-->
+                       <scope>provided</scope>
                </dependency>
                <dependency>
                        <groupId>joda-time</groupId>
```
Then run the following while inside the `storm` directory:
```
mvn clean compile package
```

Since the topology code can run on any one of the nodes in the cluster, we need to ensure that the sample input file and the log directory is a location that can be accessed from any of the nodes. **Hence please ensure that you place the input file in /scratch on the turing head node and set log location to /scratch/topo-log**. Also ensure that you pass the first argument as **C and not L**. 

E.G:
```
/opt/storm/apache-storm-1.0.2/bin/storm jar target/iot-bm-storm-0.1-jar-with-dependencies.jar in.dream_lab.bm.stream_iot.storm.topo.apps.SampleTopology C IdentityTopology /scratch/newtestfile JSON-210 0.001 /scratch/topo-log dummy test
```
