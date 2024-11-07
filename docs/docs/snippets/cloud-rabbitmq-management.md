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

On non-magento-cloud instances use:

```
grep amqp -A 7 app/etc/env.php
```

## Set Variables

Set the path of your magrnto `env.php` file.

```
export ENVFILE=app/etc/env.php
```

Set the below variables for RabbitMQ conectivity.

```
export RABBITHOST=$(grep amqp -A 7 ${ENVFILE} | grep host | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/['][,]//");

export RABBITUSER=$(grep amqp -A 7 ${ENVFILE} | grep user | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/['][,]//");

export RABBITPASS=$(grep amqp -A 7 ${ENVFILE} | grep password | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/['][,]//");
```


## Connect to RabbitMQ in MCloud

### List Exchanges

```
rabbitmqadmin -u ${RABBITUSER} -p ${RABBITPASS} -H ${RABBITHOST} list exchanges
```

### List Vhosts

```
rabbitmqadmin -u ${RABBITUSER} -p ${RABBITPASS} -H ${RABBITHOST} list vhosts
```

### List Queues

```
rabbitmqadmin -u ${RABBITUSER} -p ${RABBITPASS} -H ${RABBITHOST} list queues
```

Specify Columns

```
rabbitmqadmin -u ${RABBITUSER} -p ${RABBITPASS} -H ${RABBITHOST} list queues vhost name node messages message_stats.publish_details.rate
```

Long Listing

```
rabbitmqadmin -u ${RABBITUSER} -p ${RABBITPASS} -H ${RABBITHOST} list queues -f long -d 3 list queues
```


### List Bindings

```
rabbitmqadmin -u ${RABBITUSER} -p ${RABBITPASS} -H ${RABBITHOST} list bindings
```

## Additional Info.

In the above senario, the admin port is running on TCP port 80/HTTP.
But if in your case the rabbitMQ admin port is running on TCP port 443/HTTPS then add below option:

```
--port 443 --ssl
```

for ex:

```
rabbitmqadmin -u ${RABBITUSER} -p ${RABBITPASS} -H ${RABBITHOST} --port 443 --ssl list queues
```

## References

URL's used:

 - https://www.rabbitmq.com/docs/management-cli
