# RabbitMQ Management

To get the RabbitMQ connection credentials, Refer:

URL: https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/services-yaml.html?lang=en

or execute below comand

```
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Which will provide you mething like:

```
   "rabbitmq" : [
      {
         "username" : "mcloudprojectID_stg2",
         "vhost" : "mcloudprojectID_stg2",
         "port" : "5672",
         "host" : "localhost",
         "scheme" : "amqp",
         "password" : "Password_of_your_RabbitMQ"
      }
   ],
```

## Connect to RabbitMQ in MCloud

Lets set the rabbitmq password as:

```
export RABBITPASS=Password_of_your_RabbitMQ
```

### List Exchnages

```
rabbitmqadmin -u mcloudprojectID_stg2 -p ${RABBITPASS} -H localhost list exchanges
```

### List Vhosts

```
rabbitmqadmin -u mcloudprojectID_stg2 -p ${RABBITPASS} -H localhost list vhosts
```

### List Queues

```
rabbitmqadmin -u mcloudprojectID_stg2 -p ${RABBITPASS} -H localhost list queues
```

### List Bindings

```
rabbitmqadmin -u mcloudprojectID_stg2 -p ${RABBITPASS} -H localhost list bindings
```
