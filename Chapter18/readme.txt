sudo -i
sh gradlew build && docker-compose build
kubectl create namespace hands-on
kubectl config set-context $(kubectl config current-context) --namespace=hands-on
kubernetes/scripts/deploy-dev-env.bash 


 oc new-app --name review-service  --insecure-registry  https://github.com/woyaofuwu/Hands-On-Microservices-with-Spring-Boot-and-Spring-Cloud.git --context-dir=Chapter18/microservices/review-service 

oc new-app  --name dockerbuild --strategy docker  https://github.com/woyaofuwu/Hands-On-Microservices-with-Spring-Boot-and-Spring-Cloud.git --context-dir=Chapter18/microservices/review-service

# OpenShift - 部署MySQL主从复制
 oc create -f https://raw.githubusercontent.com/sclorg/mysql-container/master/examples/replica/mysql_replica.json
