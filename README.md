## Demo using Job Queue with RabbitMQ and Go

### Running
 - Start RabbitMQ with Docker

    ```console
    docker-compose up
    ```

 - Start Publisher

    ```console
    cd producer
    ```
    and

    ```console
    go run main.go
    ```
    
    send payload

    ```curl
    curl -X POST \
    http://localhost:3000/api/send \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    -H 'postman-token: d8e4a3cc-fcad-2d07-29f7-ad2f47ba3e66' \
    -d '{
        "from": "Wuriyanto",
            "content":{
                "header": "This is Message",
                "body": "Hello Rabbit"
            }
        }'
    ```

 - Start Consumer

    ```console
    cd producer
    ```
    and

    ```console
    go run main.go
    ```

    you'll see messages like this

    ```console
    {Wuriyanto {This is Message Hello Rabbit}}
    {Wuriyanto {This is Message 2 Hello Rabbit}}
    {Wuriyanto {This is Message 3 Hello Rabbit}}
    {Wuriyanto {This is Message 4 Hello Rabbit}}
    ```

### MQTT

Enable `RabbitMQ MQTT PLUGIN`

```shell
$ docker exec -it df8179e5e561 /bin/bash
$ rabbitmq-plugins enable rabbitmq_mqtt
```

Set `exchange` to `amq.topic`
```go
ch.Publish("amq.topic", q, false, false, payload)
```

Subscribe using `Mosquitto` https://mosquitto.org/

```shell
$ mosquitto_sub -h localhost -t 'wurys/one' -u admin -P Admin123
```