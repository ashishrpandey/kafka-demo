
## Deploy zookeeper statefulset, service and headless service

	kubectl apply -f zookeeper.yaml 


## Deploy kafka statefulset and headless service

	kubectl apply -f kafka.yaml 

## Create a topic
Run a pod, move into the pod and run kafka-topics.sh script to create the topic

	kubectl run -ti --image=gcr.io/google_containers/kubernetes-kafka:1.0-10.2.1 createtopic --restart=Never --rm -- kafka-topics.sh --create --topic test --zookeeper zk-cs.default.svc.cluster.local:2181 --partitions 1 --replication-factor 3

## Run the consumer process

	kubectl run -ti --image=gcr.io/google_containers/kubernetes-kafka:1.0-10.2.1 consume --restart=Never --rm -- kafka-console-consumer.sh --topic test --bootstrap-server kafka-0.kafka-hs.default.svc.cluster.local:9093

## In another terminal Run the Producer process 

	kubectl run -ti --image=gcr.io/google_containers/kubernetes-kafka:1.0-10.2.1 produce --restart=Never --rm -- kafka-console-producer.sh --topic test --broker-list kafka-0.kafka-hs.default.svc.cluster.local:9093,kafka-1.kafka-hs.default.svc.cluster.local:9093,kafka-2.kafka-hs.default.svc.cluster.local:9093 done;



Reference: 
https://kow3ns.github.io/kubernetes-kafka/manifests/

