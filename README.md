# demo-app
```
cd app
GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o main main.go
cd ..
docker build -t demo-app .
docker run -p 8080:8080 demo-app
curl http://localhost:8080/
```
