
export BOOK_HOME=~/github/Hands-On-Microservices-with-Spring-Boot

sudo -i
sh gradlew build && docker-compose build
kubectl create namespace hands-on
kubectl config set-context $(kubectl config current-context) --namespace=hands-on
kubernetes/scripts/deploy-dev-env.bash 



kubectl create configmap config-repo --from-file=config-repo/ --save-config


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

 oc new-app --name review-service  --insecure-registry  https://github.com/woyaofuwu/Hands-On-Microservices-with-Spring-Boot-and-Spring-Cloud.git --context-dir=Chapter18/microservices/review-service 

oc new-app  --name dockerbuild --strategy docker  https://github.com/woyaofuwu/Hands-On-Microservices-with-Spring-Boot-and-Spring-Cloud.git --context-dir=Chapter18/microservices/review-service

# OpenShift - 部署MySQL主从复制
 oc create -f https://raw.githubusercontent.com/sclorg/mysql-container/master/examples/replica/mysql_replica.json
