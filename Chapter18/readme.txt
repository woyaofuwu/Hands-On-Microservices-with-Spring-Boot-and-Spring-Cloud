sudo -i
sh gradlew build && docker-compose build
kubectl create namespace hands-on
kubectl config set-context $(kubectl config current-context) --namespace=hands-on
kubernetes/scripts/deploy-dev-env.bash 