# es-tls
elasticsearch docker-compose tls

## create certificates

```
docker-compose -f create-certs.yml run --rm create_certs
```

## start the cluster

```
docker-compose up
```

## set users and passwords

```
docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords \
auto --batch --url https://es01:9200"
```

This will generate random passwords for your cluster.
Copy them to a safe file.

## set generated password in .env file

```
ELASTIC_PASSWORD=generated_passwd
```

## restart kibana worker

Open a new terminal and dive to the project directory and run the following.

Stop the kibana container

```
docker-compose stop -t 1 kib01
```

Edit the .env file and save the new passwords

Recreate the container

```
docker-compose up --no-start kib01
```

Start the container again

```
docker-compose start kib01
```

That's all, you now have a 3 node cluster with kibana with Elastic security and TLS enabled.






