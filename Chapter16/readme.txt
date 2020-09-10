
export BOOK_HOME=~/github/Hands-On-Microservices-with-Spring-Boot
echo $BOOK_HOME
 kubectl apply -k kubernetes/services/overlays/dev 
# -k Kustomize


wget https://github.com/docker/compose/releases/download/1.27.0/docker-compose-Linux-x86_64
mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

sudo -i
sh gradlew build && docker-compose build

 ./gradlew build
sudo docker-compose build

kubectl create namespace hands-on
kubectl config set-context $(kubectl config current-context) --namespace=hands-on

kubectl create configmap config-repo --from-file=config-repo/ --save-config
kubectl describe configmap/config-repo

kubectl create secret generic config-server-secrets \
  --from-literal=ENCRYPT_KEY=my-very-secure-encrypt-key \
  --from-literal=SPRING_SECURITY_USER_NAME=dev-usr \
  --from-literal=SPRING_SECURITY_USER_PASSWORD=dev-pwd \
  --save-config

kubectl create secret generic config-client-credentials \
--from-literal=CONFIG_SERVER_USR=dev-usr \
--from-literal=CONFIG_SERVER_PWD=dev-pwd --save-config


docker pull mysql:5.7
docker pull mongo:3.6.9
docker pull rabbitmq:3.7.8-management
docker pull openzipkin/zipkin:2.12.9


sudo docker login 
sudo docker image tag hands-on/recommendation-service docker.io/robin9999/recommendation-service
sudo docker push docker.io/robin9999/recommendation-service

sudo docker image tag hands-on/product-service docker.io/robin9999/product-service
sudo docker push docker.io/robin9999/product-service

sudo docker image tag hands-on/review-service docker.io/robin9999/review-service
sudo docker push docker.io/robin9999/review-service

sudo  docker image tag hands-on/product-composite-service docker.io/robin9999/product-composite-service
sudo  docker push docker.io/robin9999/product-composite-service

sudo  docker image tag hands-on/config-server docker.io/robin9999/config-server
sudo  docker push docker.io/robin9999/config-server

sudo  docker image tag hands-on/auth-server docker.io/robin9999/auth-server
sudo  docker push docker.io/robin9999/auth-server


sudo  docker image tag hands-on/gateway docker.io/robin9999/gateway
sudo  docker push docker.io/robin9999/gateway

kubectl apply -k kubernetes/services/overlays/dev

 oc logs -f pod/

kubectl delete deployment/mongodb #无pvc默认无法写导致无法确定mongodb
 #改用ocp自带mongodb (使用模板创建)
kubectl delete dc/mongodb #模板创建无法使用，重新使用原来yaml，借用模板创建的PVC
 oc delete service/mongodb
kubectl apply -f kubernetes/services/overlays/dev/mongodb-dev.yml

kubectl delete deployment/recommendation
kubectl apply -f kubernetes/services/base/recommendation.yml

kubectl delete deployment/review
kubectl apply -f kubernetes/services/base/review.yml

问题：默认ocp中不能使用80端口f->增加scc anyuid 到 default 删除deployment再重建
oc adm policy add-scc-to-user anyuid  -z default -n hands-on
kubectl delete deployment/auth-server
kubectl apply -f kubernetes/services/base/auth-server.yml

kubectl delete deployment/product
kubectl apply -f kubernetes/services/base/product.yml

kubectl delete deployment/product-composite
kubectl apply -f kubernetes/services/base/product-composite.yml

kubectl wait --timeout=600s --for=condition=ready pod --all
kubectl get pods -o json | jq .items[].spec.containers[].image

EXEC="docker run --rm -it --network=my-network alpine"
kubectl get pod -l app=product -o jsonpath='{.items[*].spec.containers[*].image} '
kubectl get pod -l app=product -w

change yaml  
kubectl apply -k kubernetes/services/overlays/prod

kubectl set image deployment/product pro=hands-on/product-service:v3
kubectl set image deployment/product pro=hands-on/product-service:v2

kubectl rollout history deployment product
kubectl rollout history deployment product --revision=2
kubectl rollout undo deployment product --to-revision=2

 oc new-app --name review-service  --insecure-registry  https://github.com/woyaofuwu/Hands-On-Microservices-with-Spring-Boot-and-Spring-Cloud.git --context-dir=Chapter18/microservices/review-service 

oc new-app  --name dockerbuild --strategy docker  https://github.com/woyaofuwu/Hands-On-Microservices-with-Spring-Boot-and-Spring-Cloud.git --context-dir=Chapter18/microservices/review-service

# OpenShift - 部署MySQL主从复制
 oc create -f https://raw.githubusercontent.com/sclorg/mysql-container/master/examples/replica/mysql_replica.json
