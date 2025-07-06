
<!-- Setup Dotnet Project Env: -->

<!-- Create: -->
dotnet new webapp

<!-- Run: -->
dotnet run



<!-- Setup Docker Env: -->

<!-- Initialize a project with the files necessary to run the project in a container -->

docker init  

<!-- Build Docker Container: -->

docker build -t dotnetwebapp .

<!-- Run docker image on specific port -->
docker run -p 8080:80 *dockerimageID* .

docker run -p 8080:80 feb330c48f28 .

<!-- List docker Image -->
docker image ls

<!-- List docker running continers  -->
docker container ls 

<!-- Stop Container -->
docker stop *Container ID*

<!-- Remote docker image -->
docker rmi *imageID*

<!-- Remote docker image forcefully -->
docker rmi *imageID* -f


<!-- Build the project by running the following command, swapping out DOCKER_USERNAME with your username. -->


 docker build -t DOCKER_USERNAME/getting-started-todo-app .

 <!-- For example, if your Docker username was mobydock, you would run the following: -->


 docker build -t mobydock/getting-started-todo-app .


 <!-- To verify the image exists locally, you can use the docker image ls command: -->


 docker image ls
Y
<!-- ou will see output similar to the following: -->

REPOSITORY                          TAG       IMAGE ID       CREATED          SIZE
mobydock/getting-started-todo-app   latest    1543656c9290   2 minutes ago    1.12GB
...


<!-- To push the image, use the docker push command. Be sure to replace DOCKER_USERNAME with your username: -->

 docker push DOCKER_USERNAME/getting-started-todo-app



At first, you need to delete/stop the running container that is using your image(which one you want to remove).

docker ps -a: To see all the running containers in your machine.
docker stop <container_id>: To stop a running container.
docker rm <container_id>: To remove/delete a docker container(only if it stopped).
docker image ls: To see the list of all the available images with their tag, image id, creation time and size.
docker rmi <image_id>: To delete a specific image.
docker rmi -f <image_id>: To delete a docker image forcefully
docker rm -f (docker ps -a | awk '{print$1}'): To delete all the docker container available in your machine
docker image rm <image_name>: To delete a specific image
To remove the image, you have to remove/stop all the containers which are using it.

docker system prune -a: To clean the docker environment, removing all the containers and images.


<!-- Docker File Explain: -->
*********************

# Step 1: Specify your base image
FROM mcr.microsoft.com/dotnet/core/sdk:3.1

# Step 2: Copy project file to the /src container folder
WORKDIR /src
COPY ["MyCoolApp/MyCoolApp.csproj", "MyCoolApp/"]

# Step 3: Run a restore to download dependencies
RUN dotnet restore "MyCoolApp/MyCoolApp.csproj"

# Step 4: Copy app code to the container
COPY . .
WORKDIR "/src/MyCoolApp"

# Step 5: Build the app in Release mode
RUN dotnet build "MyCoolApp.csproj" -c Release -o /app

# Step 6: Publish the application
RUN dotnet publish "MyCoolApp.csproj" -c Release -o /app

# Step 7: Expose port 80 in the container
EXPOSE 80

# Step 8: Move the /app folder and run the compiled app
WORKDIR /app
ENTRYPOINT ["dotnet", "MyCoolApp.dll"]