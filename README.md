# demo-app
### Build and run docker image locally
```
cd app
GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o main main.go
cd ..
docker build -t demo-app .
docker run -p 8080:8080 demo-app
curl http://localhost:8080/
```
### Set account number
```
export ACCOUNT_NUMBER=$(aws sts get-caller-identity --output json | jq -r ".Account" | tr -d '[:space:]')
```
### Build, push, and install helm chart
Log in to ecr and create helm repo
```
aws ecr get-login-password --region us-east-1 | helm registry login \
       --username AWS --password-stdin $ACCOUNT_NUMBER.dkr.ecr.us-east-1.amazonaws.com
aws ecr create-repository --repository-name helm-demo-app-chart --region us-east-1
```
Build and push chart
```
cd chart
helm package demo-app
helm push helm-demo-app-chart-0.1.0.tgz oci://$ACCOUNT_NUMBER.dkr.ecr.us-east-1.amazonaws.com/
```
Install chart
```
helm install demo-app oci://$ACCOUNT_NUMBER.dkr.ecr.us-east-1.amazonaws.com/helm-demo-app-chart --version 0.1.0 -f values-dev.yaml
```
Access the app
```
kubectl port-forward service/demo-app 8080:8080
curl http://localhost:8080
```
