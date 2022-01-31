# Real-time-BUH-maps
Let's assume that we have 3 vehicles which have a pre-defined route (knowing some of their stop's coordinates), then we can show their real-time location on a map.

We can build the application by using the following stack: Python + Flask, Kafka and a little bit of knowledge of Web-Development (HTML, CSS + JS). 

<h1> Steps </h1>

1. Create the data by generating a couple of routes through www.geojson.io, and save the generated results as .json files.
2. Start Zookeeper and Kafka, by following the commands from [0] (of course, after downloading Kafka from Apache). 
3. Create a testing topic (see the command at [0]. And afterwards start the producer in Python. 
4. Parse the each json, by selecting only the key-value pair coordinates: [long, lat].
5. Afterwards, dump the data collected from all the json files into a dictionary that retains the id of the vehicle, a timestamp, and also the coordinates of their routes. 
6. While you're dumping the data, you also need to publish it. Once a vehicle reaches the final coordinates of their route, it goes back to where their route started. 
7. With Flask, create an app.py where you're going to render an index file, and also a Consumer.
8. Now, it's time to create the index, where personally, I combined all of the web technologies into one single file (as there wasn't much scripting/styling to do).
9. I used https://leafletjs.com/, for building the interactive map. Most of the commands can be found in Tutorials -> Leaflet Quick Start Guide.
10. Afterwards, I created a function which parses the data dictionary created in Step 5 and puts a marker for each vehicle and their stops. 

<h3> I ended up with creating a small application, that renders the location of n vehicles, each being represented on a map by a marker. As a future improvement, we could take the geolocation of the personal phone (instead of a predefined route), and add the coordinates into a topic, for a further analysis (such as Google Maps' Timeline feature).  </h3> 

![alt text](https://github.com/Scarlett1309/Real-time-BUH-maps/blob/d23719fb9a40209dac63bc6fbc501e6b1d29c2bb/data/app.png)

[0] Kafka commands:

Set an environment path to the windows folder from the kafka download file (in order to run files and to not specify the main path). Then create 2 empty folders (for kafka-logs and zookeeper-logs) in the kafka home folder, and modify the path for DataDir both in server.properties and zookeeper.properties (found in kafka/config/).

- Start the Zookeeper service (which will be no longer required by Kafka): 

`bin/windows/zookeeper-server-start.bat config/zookeeper.properties`

- Start the Kafka server:

`bin/windows/kafka-server-start.bat config/server.properties`

- To create a topic, run the following command:

`kafka-topics.bat --create --topic test --bootstrap-server localhost:9092`

- View details about the topic:

`kafka-topics.bat --describe --topic test --bootstrap-server localhost:9092`

- Afterwards, run the following command in order to open the producer:

`kafka-console-producer.bat --topic test --bootstrap-server localhost:9092`

- Send a couple of random messages and afterwards open another terminal to run the consumer, in order to receive the events you've just created:

`kafka-console-consumer.bat --topic test --from-beginning --bootstrap-server localhost:9092`

(you can stop the consumer/producer anytime by pressing CTRL+C)
