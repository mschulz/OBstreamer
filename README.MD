#Processing Openbeacon Active Badge Messages
This set of packages process the messages coming from Openbeacon Active Badges via Openbeacon readers and produces a JSON description of averaged data about the Openbeacon readers and Openbeacon badges.

##Architecture
The processing of the badge data is processed by three separate process. Real-time data is stored in a Redis database (i.e., and in-memory database with data structures which simplify the task here). MQTT is used to pass messages between these processes, and to make the JSON stream available for further processing.

###Input Message Storage - handled by openbeacon\_mqtt\_2_redis.py
The process subscribes to the MQTT topic 'openbeacon/LIB/fastRaw/reader/+', where the '+' exapnds to the IP address of the reader.  Each message payload is the raw data received from each badge; it is assumed that the reader does no processing of the badge data (an Arduino does not have enough horsepower to do much more than grab messages off the air and send them over Ethernet to the message server).

The payload is then transferred to the Redis database.  The description of the Redis data structures used to hold all this data is in the file [Openbeacon-redis-documentation](docs/Openbeacon-redis-documentation.pdf)


###Averaged Message Data Production
Redis database walk -> JSON summary (handled by redis-walker.py). Subsequent processing of this data can be accomplished through a subscription to the MQTT topic 

###Removal of Aged Data
Removal of aged-out data from the Redis database (handled by ob\_redis\_cleanup.py)

