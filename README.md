# demo-app
### Build and run docker image locally
Note: this is for local testing, github action will use goreleaser to build and push the image to ecr
```
cd app
GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o main main.go
cd ..
docker build -t demo-app .
docker run -p 8080:8080 demo-app
curl http://localhost:8080/
```

### Create a secret to store Github Token
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: github-token
type: Opaque
data:
  GITHUB_TOKEN: <your-github-token>
EOF
```

### Install the app with argocd
```
kubectl apply -f argo-app-dev.yaml
```
Access the app
```
kubectl port-forward service/demo-app 8080:8080
curl http://localhost:8080
```
