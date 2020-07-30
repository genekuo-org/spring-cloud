## Reactive
./gradlew build && docker-compose build && docker-compose up -d

curl localhost:8080/actuator/health -s | jq -r .status

body='{"productId":1,"name":"product name C","weight":300, "recommendations":[ {"recommendationId":1,"author":"author 1","rate":1,"content":"content 1"}, {"recommendationId":2,"author":"author 22","rate":2,"content":"content 2"}, {"recommendationId":3,"author":"author 3","rate":3,"content":"content 3"} ], "reviews":[ {"reviewId":1,"author":"author 1","subject":"subject 1","content":"content 1"}, {"reviewId":2,"author":"author 2","subject":"subject 2","content":"content 2"}, {"reviewId":3,"author":"author 3","subject":"subject 3","content":"content 3"} ]}'

curl -X POST localhost:8080/product-composite -H "Content-Type: application/json" --data "$body"
http://localhost:15672/#/queues
guest/guest

curl localhost:8080/product-composite/1 | jq

curl -X DELETE localhost:8080/product-composite/1

docker-compose down

export COMPOSE_FILE=docker-compose-partitions.yml
docker-compose build && docker-compose up -d

body='{"productId":1,"name":"product name C","weight":300, "recommendations":[ {"recommendationId":1,"author":"author 1","rate":1,"content":"content 1"}, {"recommendationId":2,"author":"author 22","rate":2,"content":"content 2"}, {"recommendationId":3,"author":"author 3","rate":3,"content":"content 3"} ], "reviews":[ {"reviewId":1,"author":"author 1","subject":"subject 1","content":"content 1"}, {"reviewId":2,"author":"author 2","subject":"subject 2","content":"content 2"}, {"reviewId":3,"author":"author 3","subject":"subject 3","content":"content 3"} ]}'

body='{"productId":2,"name":"product name C","weight":300, "recommendations":[ {"recommendationId":1,"author":"author 1","rate":1,"content":"content 1"}, {"recommendationId":2,"author":"author 22","rate":2,"content":"content 2"}, {"recommendationId":3,"author":"author 3","rate":3,"content":"content 3"} ], "reviews":[ {"reviewId":1,"author":"author 1","subject":"subject 1","content":"content 1"}, {"reviewId":2,"author":"author 2","subject":"subject 2","content":"content 2"}, {"reviewId":3,"author":"author 3","subject":"subject 3","content":"content 3"} ]}'

curl -X POST localhost:8080/product-composite -H "Content-Type: application/json" --data "$body"

http://localhost:15672/#/queues
docker-compose down
unset COMPOSE_FILE

export COMPOSE_FILE=docker-compose-kafka.yml
docker-compose build && docker-compose up -d

docker-compose exec kafka /opt/kafka/bin/kafka-topics.sh --zookeeper zookeeper --list
docker-compose exec kafka /opt/kafka/bin/kafka-topics.sh --describe --zookeeper zookeeper --topic products
docker-compose exec kafka /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic products --from-beginning --timeout-ms 1000
docker-compose exec kafka /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic products --from-beginning --timeout-ms 1000 --partition 1

docker-compose down
unset COMPOSE_FILE

[clean up]
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q) && docker rmi $(docker images -a -q)
