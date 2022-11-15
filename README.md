# Kafka & Monk

This repository contains Monk.io template to deploy apache-kafka system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).


## Start

Set up Monk - https://docs.monk.io/docs/monk-in-10/

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk kafka repository

In order to load templates and change configuration simply use below commands: 
```bash
git clone https://github.com/monk-io/monk-kafka

```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `stack.yaml/variables` section

```yaml
  variables:
    kafka-image-tag: "7.2.1"
    zookeeper-image-tag: "7.2.1"
```

##  Template variables

| Variable | Description | Type | Example |
|----------|-------------|------|---------|
| **zookeeper-image-tag** | Zookeeper image version. | string | 7.2.1 |
| **kafka-image-tag** | Kafka image version. | string | 7.2.1 |



## Local Deployment

First clone the repository  and simply run below command after launching `monkd`:
:

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  ├─🧩 kafka-cluster/kafka-common
 │  ├─🧩 kafka-cluster/zoo-3
 │  ├─🧩 kafka-cluster/zoo-2
 │  ├─🧩 kafka-cluster/kafka-3
 │  ├─🧩 kafka-cluster/zoo-1
 │  ├─🧩 kafka-cluster/kafka-1
 │  └─🧩 kafka-cluster/kafka-2
 ├─🔗 Process groups:
 │  └─🧩 kafka-cluster/stack
 └─⚙️ Entity instances:
    ├─🧩 kafka-cluster/kafka-common/metadata
    ├─🧩 kafka-cluster/zoo-1/metadata
    ├─🧩 kafka-cluster/zoo-2/metadata
    └─🧩 kafka-cluster/zoo-3/metadata
✔ All templates loaded successfully

➜  monk list kafka-cluster

✔ Got the list
Type      Template                    Repository  Version  Tags
runnable  kafka-cluster/kafka-1       local       -        streaming, data, analytics, integration, distributed
runnable  kafka-cluster/kafka-2       local       -        streaming, data, analytics, integration, distributed
runnable  kafka-cluster/kafka-3       local       -        streaming, data, analytics, integration, distributed
runnable  kafka-cluster/kafka-common  local       -        streaming, data, analytics, integration, distributed
group     kafka-cluster/stack         local       -        -
runnable  kafka-cluster/zoo-1          local       latest   configuration, services
runnable  kafka-cluster/zoo-2          local       latest   configuration, services
runnable  kafka-cluster/zoo-3          local       latest   configuration, services


➜  monk run kafka-cluster/stack

✔ Started local/kafka-cluster/stack

```

This will start the entire kafka-cluster/stack with a Nginx reverse proxy. 


## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name kafka
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p gcp -f <path/to/your-key.json>
✔ Provider added successfully

➜  monk cluster grow -p gcp
? Cloud provider gcp
? Name of the new instance my-instance
? Tags (split by whitespace) kafka
? Region europe-central2
? Zone europe-central2-a
? Instance type e2-medium
? Number of instances (or press ENTER to use default = 1) 3
? Default disk type for gcp is HDD (pd-standard). Would you like to change it? No
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on gcp... DONE
✔ Creating node: my-instance-1 DONE
✔ Initializing node: my-instance-1 DONE
✔ Creating node: my-instance-2 DONE
✔ Initializing node: my-instance-2 DONE
✔ Creating node: my-instance-3 DONE
✔ Initializing node: my-instance-3 DONE
✔ Connecting: my-instance-1 DONE
✔ Syncing peer: my-instance-1 DONE
✔ Connecting: my-instance-2 DONE
✔ Connecting: my-instance-3 DONE
✔ Syncing peer: my-instance-2 DONE
✔ Syncing peer: my-instance-3 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).
```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  ├─🧩 kafka-cluster/kafka-common
 │  ├─🧩 kafka-cluster/zoo-3
 │  ├─🧩 kafka-cluster/zoo-2
 │  ├─🧩 kafka-cluster/kafka-3
 │  ├─🧩 kafka-cluster/zoo-1
 │  ├─🧩 kafka-cluster/kafka-1
 │  └─🧩 kafka-cluster/kafka-2
 ├─🔗 Process groups:
 │  └─🧩 kafka-cluster/stack
 └─⚙️ Entity instances:
    ├─🧩 kafka-cluster/kafka-common/metadata
    ├─🧩 kafka-cluster/zoo-1/metadata
    ├─🧩 kafka-cluster/zoo-2/metadata
    └─🧩 kafka-cluster/zoo-3/metadata
✔ All templates loaded successfully

➜  monk list kafka-cluster

✔ Got the list
Type      Template                    Repository  Version  Tags
runnable  kafka-cluster/kafka-1       local       -        streaming, data, analytics, integration, distributed
runnable  kafka-cluster/kafka-2       local       -        streaming, data, analytics, integration, distributed
runnable  kafka-cluster/kafka-3       local       -        streaming, data, analytics, integration, distributed
runnable  kafka-cluster/kafka-common  local       -        streaming, data, analytics, integration, distributed
group     kafka-cluster/stack         local       -        -
runnable  kafka-cluster/zoo-1          local       latest   configuration, services
runnable  kafka-cluster/zoo-2          local       latest   configuration, services
runnable  kafka-cluster/zoo-3          local       latest   configuration, services
➜  monk run kafka/stack

✔ Started local/kafka-cluster/stack

```

## Logs & Shell

```bash
# show Zookeeker-1 logs
➜  monk logs -l 1000 -f local/kafka-cluster/zoo-1

# show Zookeeker-2 logs
➜  monk logs -l 1000 -f local/kafka-cluster/zoo-2

# show Zookeeker-3 logs
➜  monk logs -l 1000 -f local/kafka-cluster/zoo-3

# show Kafka-1 logs
➜  monk logs -l 1000 -f local/kafka-cluster/kafka-1

# show Kafka-2 logs
➜  monk logs -l 1000 -f local/kafka-cluster/kafka-2

# show Kafka-3 logs
➜  monk logs -l 1000 -f local/kafka-cluster/kafka-3




# access shell in the container running Zookeeker-1
➜  monk shell local/kafka-cluster/zoo-1

# access shell in the container running Zookeeker-2
➜  monk shell local/kafka-cluster/zoo-1

# access shell in the container running Zookeeker-3
➜  monk shell local/kafka-cluster/zoo-1

# access shell in the container running Kafka-1
➜  monk shell local/kafka-cluster/kafka-1

# access shell in the container running Kafka-2
➜  monk shell local/kafka-cluster/kafka-2

# access shell in the container running Kafka-1
➜  monk shell local/kafka-cluster/kafka-3





```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge -x -a kafka-cluster/stack kafka-cluster/kafka-common  local/kafka-cluster/zoo-1 local/kafka-cluster/zoo-2 local/kafka-cluster/zoo-3 local/kafka-cluster/kafka-1 local/kafka-cluster/kafka-2 local/kafka-cluster/kafka-3

✔ kafka-cluster/stack purged
✔ local/kafka-cluster/zoo-1    purged
✔ local/kafka-cluster/zoo-2    purged
✔ local/kafka-cluster/zoo-3    purged
✔ local/kafka-cluster/kafka-1  purged
✔ local/kafka-cluster/kafka-2  purged
✔ local/kafka-cluster/kafka-3  purged
```