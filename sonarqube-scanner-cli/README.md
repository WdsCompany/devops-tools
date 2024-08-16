```sh

docker build -t img .

docker run -e SONAR_HOST_URL="########"  -e SONAR_TOKEN="########"  img sonar-scanner

```
