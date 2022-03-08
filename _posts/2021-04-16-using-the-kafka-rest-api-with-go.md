---
title: "Using the Kafka REST API with Go"
date: 2021-04-16 10:03:07.000000000 +02:00
categories:
- go
- Golang
- Programming
- Software
- Systems
- Technology
tags:
- go
- golang
- kafka
- software
permalink: "/using-the-kafka-rest-api-with-go/"
excerpt: "Learn how to build your own Kafka driver in Go using the Kafka API REST proxy."
layout: splash
---
This post is an adaption of a fragment extracted from my book ***Build systems with Go: everything a Gopher must know"***. You can find more details [here](https://jmtirado.net/build-systems-with-go/).

------------

Apache Kafka has demonstrated to be a scalable solution and it is widely adopted as a publish/subscribe event streaming platform. The ecosystem around Kafka is vast and rich and you have probably used one of its many solutions.

Kafka uses its own protocol over  [TCP](https://docs.confluent.io/3.0.0/kafka-rest/docs/index.html) that can be implemented in any programming language. In the case of Go, there are some available implementations. The official one provided by [Confluent](https://github.com/confluentinc/confluent-kafka-go) is a wrapper around the librdkafka library. Others like the one offered by [Segmentio](https://github.com/segmentio/kafka-go) are Go native and do not require any additional libraries.

The utilization of wrappers around libraries may not be the most convenient solution for all scenarios, particularly if you fully trust Go. In the case of native implementations, not all the operations offered by the official Kafka driver are  available. You can always implement your driver following the specification mentioned above... But be honest. We all know you are not going to do that.

A potential solution is to use the REST API proxy offered by Kafka ([here is the API documentation](https://docs.confluent.io/3.0.0/kafka-rest/docs/index.html)). In this post, I will show you how to implement a producer and a consumer using this API. To do so, I will only use the Go standard `net/http` package.

These examples assume the Kafka REST Proxy is running and listening to `localhost:8082`. You can find some help on how to install a local Kafka using a Docker compose in the official [guide](https://docs.confluent.io/platform/current/quickstart/ce-docker-quickstart.html#ce-docker-quickstart) or you can check my [repo](https://github.com/juanmanuel-tirado/savetheworldwithgo/blob/master/18_kafka/README.md) for additional help. The code I will show here can be found there.

## Producer

First, we have to know the REST method for message publishing. After checking the API specification, we find out that we have to use the POST method `/topics/<name>/partitions/<number>`. Where `<name>` and `<number>` are the topic name and the corresponding partition respectively. The message must be attached to the body request. This method expects the encoding format to be specified in the request `Content-Type` header. The API admits data encoding in JSON, Avro, binary, and Protobuf formats. In these examples, we use JSON encoding with the value  `application/vnd.kafka.json.v2+json` for our header. On how to specify other formats see the official documentation. The following object contains two messages (records) to be published.

```
{
    "records": 
    [
        {"value":{"name":"John","email":"john@gmail.com"}},
        {"value":{"name":"Mary","email":"mary@email.com"}}
    ]
}
```

Note that for simplicity we are only indicating the content of our messages, but other fields such as the key or the timestamp can be set. Check the reference for more details.

Now, we can send this object in the body of a `POST` request to the API using the code below.

<script src="https://gist.github.com/juanmanuel-tirado/46b5608205e8d06fb6f2f44c90e5e5ca.js"></script>

The `BuildBody` function is a helper to generate the body of the request. Note that we set the corresponding `Content-Type` header with the `CONTENT_TYPE` constant to the `Post` function. Finally, the program prints the body from the server response. If we run this code, we can see that the  body of the server response contains information about the partitions and offsets of every written message. Notice that these values will vary according to your topic status.

```
{"records": [{"value":{"name":"John","email":"john@gmail.com"}},{"value":{"name":"Mary","email":"mary@email.com"}}]}
200 OK
{"offsets":[{"partition":0,"offset":165,"error_code":null,"error":null},{"partition":0,"offset":166,"error_code":null,"error":null}],"key_schema_id":null,"value_schema_id":null}
```


## Consumer

Consuming messages from the API REST requires more steps than producing them.  To get a message a consumer must: 1) create a new consumer instance, 2) subscribe to a topic and group, 3) fetch the messages, and finally 4) delete the instance if no more messages are going to be consumed. The program is split into four pieces corresponding to these steps.

![Kafka consumer API REST communication diagram]({{ source }}/assets/2021/04/kafkadiagram-300x137.png "Kafka consumer API REST communication diagram")

To create a new consumer, we use the `POST` method `/consumers/testGroup` where `testGroup` is the name of the consumers' group. The body request contains the name of the consumer (`testConsumer`) and the format to be used by the messages (`json`). 

```
{"name":"testConsumer", "format": "json"}
```

The response body contains the consumer id and the base URI to be used by this consumer. This can be achieved with the following piece of code. The `DoHelper` is a helper function for the `POST` requests.

<script src="https://gist.github.com/juanmanuel-tirado/b0c2e64f730226c6b58c0b7162188c3e.js"></script>

Next, the consumer is subscribed to the topic `helloTopic` where the producer sent the messages. The target `POST` method matches the base URI received in the response from the previous creation of the consumer instance extended with the suffix `subscription`. In our case, `/consumers/testGroup/instances/testConsumer/subscription`. The answer's body contains the list of topics this consumer requested to be subscribed to. The response returns a 204 code indicating a correct response without a  body.

<script src="https://gist.github.com/juanmanuel-tirado/1472501bd559c7dc2fc8c969df0c1dd3.js"></script>

Now the consumer is ready to receive records from the topics it has been subscribed to. A `GET` request to the base URI with the `records` suffix will return any available record. In this case, `/consumers/testGroup/instances/testConsumer/records`. Note that the `Accept` header must be set with the corresponding content type we want the incoming messages to be encoded (JSON in this case). The response body will contain the available messages. if no messages are available at the time of sending the request, the returned response will be empty. Additional query parameters are `timeout` to specify the maximum time the server will spend fetching records and `max_bytes` with the maximum size of the returned records.

<script src="https://gist.github.com/juanmanuel-tirado/b0b2a62fd83d5fe5ff2e733a269c80e9.js"></script>

Finally, we delete the consumer instance to release resources with a `POST` request to `/consumers/testGroup/instances/testConsumer`. The body from the response is empty with a 204 status.

<script src="https://gist.github.com/juanmanuel-tirado/d25ee37c18f797b2a83c212b52f75b07.js"></script>

By running the program we can see the requests, responses, and received bodies during the process.

```
-->Call http://localhost:8082/consumers/testGroup
-->Body {"name":"testConsumer", "format": "json"}
<--Response 200 OK
<--Body {"instance_id":"testConsumer","base_uri":"http://rest-proxy:8082/consumers/testGroup/instances/testConsumer"}
-->Call http://localhost:8082/consumers/testGroup/instances/testConsumer/subscription
-->Body {"topics":["helloTopic"]}
<--Response 204 No Content
-->Call http://localhost:8082/consumers/testGroup/instances/testConsumer/records
<--Response 200 OK
<--Body [{"topic":"helloTopic","key":null,"value":{"name":"John","email":"john@gmail.com"},"partition":0,"offset":179},{"topic":"helloTopic","key":null,"value":{"name":"Mary","email":"mary@email.com"},"partition":0,"offset":180}]
-->Call http://localhost:8082/consumers/testGroup/instances/testConsumer
<--Response 204 No Content
```

Note that in this case, we are not modifying the consumer offset. Calling the `POST` method at `/consumers/testGroup/instances/testConsumer/positions` with the next offset indicated in the body prepares the consumer for the next batch of messages. For our example where the latest record had offset 180, we could send the following body to set the next offset to 181.

```
{
  "offsets": [
    {
      "topic": "helloTopic",
      "partition": 0,
      "offset": 181
    }
  ]
}
```


## Summary

In certain situations, it is not possible nor convenient to use Kafka drivers. In these situations, the Kafka API REST Proxy can be a good solution. In this case, I have shown the steps to implement a producer and consumer in Go using the standard `net/http` package. I hope you find it useful.

Thanks for reading.
