# avro schema evolution examples

A very basic example of how to evolve an avro schema (assumes backwards
compatibility)

### setup

The [confluent avro schema registry](https://github.com/confluentinc/schema-registry) should be installed and running. The [confluent cli](https://github.com/confluentinc/confluent-cli) tool make this painless.

```bash
$ confluent start schema-registry
Starting zookeeper
zookeeper is [UP]
Starting kafka
kafka is [UP]
Starting schema-registry
schema-registry is [UP]

$ confluent status schema-registry
schema-registry is [UP]
kafka is [UP]
zookeeper is [UP]
```

Ensure [jq](https://stedolan.github.io/jq/download/) is installed and available within your `PATH`

### example registration

```bash
bin/register schemas/test/s001.avsc test http://localhost:8081
# {"id":1}
```

### evolve all schemas

```bash
SUBJECT=test
for VERSION in $(seq -f "%03g" 1 10); do
    bin/register "schemas/test/s${VERSION}.avsc" "$SUBJECT" "http://localhost:8081"
done
```
