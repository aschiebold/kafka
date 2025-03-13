Fix for the errors from running Kafka on Java 8 update 441. If you're following the the quickstart (https://kafka.apache.org/quickstart) here is how to fix the issues.

# "Generate a Cluster UUID"

KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
Error: VM option 'UseG1GC' is experimental and must be enabled via -XX:+UnlockExperimentalVMOptions.
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.

FIX:
Edit line 280 in kafka-run-class.sh: 

if [ -z "$KAFKA_HEAP_OPTS" ]; then
  KAFKA_HEAP_OPTS="-Xmx256M -XX:+UnlockExperimentalVMOptions"
fi

# Start the Kafka Server

bin/kafka-server-start.sh config/kraft/reconfig-server.properties
Error: VM option 'UseG1GC' is experimental and must be enabled via -XX:+UnlockExperimentalVMOptions.
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.

FIX:
Edit line 29 in kafka-server-start.sh:

if [ "x$KAFKA_HEAP_OPTS" = "x" ]; then
    export KAFKA_HEAP_OPTS="-Xmx1G -Xms1G -XX:+UnlockExperimentalVMOptions"
fi

# Step 4: Write some events into the topic

bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
Error: VM option 'UseG1GC' is experimental and must be enabled via -XX:+UnlockExperimentalVMOptions.
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.

FIX:
Edit line 18 in kafka-console-producer.sh

if [ "x$KAFKA_HEAP_OPTS" = "x" ]; then
    export KAFKA_HEAP_OPTS="-Xmx512M -XX:+UnlockExperimentalVMOptions"
fi

# Step 5: Read the events

bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
Error: VM option 'UseG1GC' is experimental and must be enabled via -XX:+UnlockExperimentalVMOptions.
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.

FIX: 
Edit line 18 in kafka-console-consumer.sh

if [ "x$KAFKA_HEAP_OPTS" = "x" ]; then
    export KAFKA_HEAP_OPTS="-Xmx512M -XX:+UnlockExperimentalVMOptions"
fi

# Step 6: Import/export your data as streams of events with Kafka Connect

bin/connect-standalone.sh config/connect-standalone.properties config/connect-file-source.properties config/connect-file-sink.properties
Error: VM option 'UseG1GC' is experimental and must be enabled via -XX:+UnlockExperimentalVMOptions.
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.

FIX: 
Edit line 30 in connect-standalone.sh

if [ "x$KAFKA_HEAP_OPTS" = "x" ]; then
  export KAFKA_HEAP_OPTS="-Xms256M -Xmx2G -XX:+UnlockExperimentalVMOptions"
fi
