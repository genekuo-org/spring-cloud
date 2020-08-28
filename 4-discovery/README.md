./gradlew build && docker-compose build

./test-all.sh start

docker-compose up -d --scale review=3
http://localhost:8761

docker-compose logs -f review

curl -H "accept:application/json" localhost:8761/eureka/apps -s | jq -r '.applications.application[].instance[].instanceId'

curl localhost:8080/product-composite/2 -s | jq -r .serviceAddresses.rev

docker-compose logs -f review

docker-compose up -d --scale review=2

[timeout]
curl localhost:8080/product-composite/2 -m 2

[Eureka crash]
docker-compose up -d --scale review=2 --scale eureka=0

curl localhost:8080/product-composite/2 -s | jq -r .serviceAddresses.rev

[Review crash]
docker-compose up -d --scale review=1 --scale eureka=0

curl localhost:8080/product-composite/2 -s | jq -r .serviceAddresses.rev

[extra product instance]
docker-compose up -d --scale review=1 --scale eureka=0 --scale product=2

curl localhost:8080/product-composite/2 -s | jq -r .serviceAddresses.pro

[Eureka back]
docker-compose up -d --scale review=1 --scale eureka=1 --scale product=2

curl localhost:8080/product-composite/2 -s | jq -r .serviceAddresses

docker-compose down

[clean up]
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
docker rmi $(docker images -a -q)

demo.microservices
