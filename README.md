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

> /path/to/input/file - The path to the input file. The file content should be a CSV as described in

> Log ID - Unique ID that is used to create the log files (log file name uses this). Ensure this is changed each time you run the code so that a separate log file is created

> Scale factor - A floating point value that is the inverse of the speed up that you require. If you want 100X speedup, then set this value to (1/100) = 0.01. For 10X set this to 0.1.

> /path/to/log/dir - Path to directory where logs need to be stored

> dummy test - Leave these arguments as is.

E.G:
```
java -jar target/iot-bm-storm-0.1-jar-with-dependencies.jar L IdentityTopology /home/dreamlab2/newtestfile JSON-210 0.001 /home/dreamlab2/Desktop/log dummy test
```
 
## Running code on the cluster
