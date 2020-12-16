# es-tls
elasticsearch docker-compose tls

## create certificates

Create certs dir in project directory and run docker image to create certificates. It's based on `instances.yml` file were you configure the number of nodes you want for your cluster.
We're using as referent [get-started-docker](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html) from Elasticsearch documentation.

```
$ mkdir certs
$ docker-compose -f create-certs.yml run --rm create_certs
```
You now have the certificates tu run on the nodes and kibana.


## start the cluster

The cluster will start and some errors will appear as passwords aren't set yet.

```
docker-compose up
```

## set users and passwords

As we've secured our cluster, we need to generate the passwords for the diferent elasticsearch users and roles. Open a new terminal and dive to the project directory, then run:

```
docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords \
auto --batch --url https://es01:9200"
```

This will generate random passwords for your cluster.
Copy them to a safe file.

## set generated password in .env file

Edit the `.env` file and put the generated password of the elasticsearch user.

```
ELASTIC_PASSWORD=generated_passwd
```

## restart kibana worker

We need to restart kibana worker so it gets the new password from the edited `.env` file.

Stop the kibana container

```
docker-compose stop -t 1 kib01
```

Edit the .env file and save the new passwords if not done yet.

Recreate the container

```
docker-compose up --no-start kib01
```

Start the container again

```
docker-compose start kib01
```

That's all, you now have a 3 node cluster with kibana with Elastic security and TLS enabled.






