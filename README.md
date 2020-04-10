This tutorial explains how to Deploy Kafka and zookeeper on Kubernetes. 

## Prerequisite: 
- Setup a multi-node kubernetes cluster up and running with a functioning kubectl.
- If you dont have a kubernetes cluster, set it up by following the instructions in the link below: 

- https://github.com/ashishrpandey/kubernetes-training/blob/master/00installation.md

## Lets start
Login to the master node.
Clone this repository ..

	git clone https://github.com/ashishrpandey/kafka-demo
	cd kafka-demo/
	

## Deploy zookeeper statefulset, zookeeper service and a headless service

	kubectl apply -f zookeeper.yaml 


## Deploy kafka statefulset and a headless service

	kubectl apply -f kafka.yaml 

## Create a topic
Run a pod, move into the pod and run kafka-topics.sh script to create the topic

	kubectl run -ti --image=gcr.io/google_containers/kubernetes-kafka:1.0-10.2.1 createtopic --restart=Never --rm -- kafka-topics.sh --create --topic test --zookeeper zk-cs.default.svc.cluster.local:2181 --partitions 1 --replication-factor 3

## Run the consumer process

	kubectl run -ti --image=gcr.io/google_containers/kubernetes-kafka:1.0-10.2.1 consume --restart=Never --rm -- kafka-console-consumer.sh --topic test --bootstrap-server kafka-0.kafka-hs.default.svc.cluster.local:9093

## In another terminal Run the Producer process 

	kubectl run -ti --image=gcr.io/google_containers/kubernetes-kafka:1.0-10.2.1 produce --restart=Never --rm -- kafka-console-producer.sh --topic test --broker-list kafka-0.kafka-hs.default.svc.cluster.local:9093,kafka-1.kafka-hs.default.svc.cluster.local:9093,kafka-2.kafka-hs.default.svc.cluster.local:9093 done;

### Note:
In this tutorial we are using non-persistent volume. This volume gets deleted with the deletion of the pod. So you should always run kafka/zookeeper statefulSets with the persistentVolumeClaimTemplate (See the commented code in yaml files) with appropriate persistent volumes.

### Reference: 
https://kow3ns.github.io/kubernetes-kafka/manifests/

