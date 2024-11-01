Book CNSiA - catalog-service
> gradle test
> gradle test --tests BookValidationTests
> gradle bootRun
> gradlew bootBuildImage
> docker images catalog-service:0.0.1-SNAPSHOT
> docker run --rm --name catalog-service -p 8080:8080 catalog-service:0.0.1-SNAPSHOT
> curl http://localhost:8080/
> gradle test --tests CatalogServiceCnsiaGrdApplicationTests
> gradle test --tests BookControllerMvcTests
> gradle test --tests BookJsonTests


#Test 
http POST :9001/books author="Jon Snow" title="" isbn="123ABC456Z" price=9.90
curl -H 'Content-Type: application/json' -X POST -d '{"author":"Lyra Silvestart", "title":"Northen Lights", "isbn":"1234567801", "price":9.90}' localhost:9001/books -v

curl -H 'Content-Type: application/json' -X POST -d '{"author":"Jon Snow", "title":"", "isbn":"123ABC456Z", "price":9.90}' localhost:9001/books -v

#Test Docker and K8s
0. Build image using buildpacks
   ./gradlew bootBuildImage
   ./mvnw -Pnative spring-boot:build-image  -DskipTests #Err tom, jdk17
   ./mvnw spring-boot:build-image -DskipTests #Command default CNSIA - unable to get dependency BellSoft Liberica JRE. jdk17

#Dockerfile
1. mvn package
2. docker build -t catalog-service:0.0.1-SNAPSHOT . #Maven => OK: catalog-service  0.0.1-SNAPSHOT
   docker images catalog-service:0.0.1-SNAPSHOT
3. docker run --rm --name catalog-service -p 8080:8080 catalog-service:0.0.1-SNAPSHOT #OK curl localhost:8080/
4. k3d image import catalog-service:0.0.1-SNAPSHOT  -c mycluster #OK
   (minikube image load catalog-service:0.0.1-SNAPSHOT)
#
5. kubectl create deployment catalog-service --image=catalog-service:0.0.1-SNAPSHOT #OK
6. kubectl get deployment #OK
7. kubectl get pod #OK
8. kubectl expose deployment catalog-service --name=catalog-service --port=8080 #OK
9. kubectl get service catalog-service #OK
10. kubectl port-forward service/catalog-service 8000:8080 #OK curl localhost:8000/

12. DELETE: kubectl get service catalog-service
            kubectl delete deployment catalog-service
#Check images en k3d
docker exec k3d-mycluster-server-0 crictl images
##Check images en k3d
kubectl logs deployment/catalog-service

#Test actions
