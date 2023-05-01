## 1. Base Image
## Backend And Client Base Image
 node:19-alpine - Alpine  is much smaller than most distribution base images and thus leads to much slimmer images in general.
 ## Database
  mongo - Itis the available image for Mongo DB

## 2. Dockerfile Directives

### _Client_

The client docker file implemeted using using multi-stage builds because it is helps achieve the following:

- Faster builds as by separating the build stage from the production stage, only rebuild the parts that have changed.
- Improved security: it ensures that the production image does not include any development tools or libraries that may introduce security vulnerabilities.
- Simpler Dockerfile: separing the stages help create a Dockerfile which is simple and easier to read and maintain.
- Smaller and efficient images by separating the build dependencies from the runtime dependencies.
- 
##### Build Stage

`WORKDIR` directive sets the working directory for the container to /client.

 `COPY` directive copies the package.json and package-lock.json files to the /client directory which are required to install the necessary dependencies for the application.

The `RUN` directive is used to execute the following commands; `npm install --only=production` to installs only the production dependencies and also clears the npm cache and removes any temporary files using `npm cache clean --force` and `rm -rf /tmp/*` respectively.

The `COPY` directive is the used to copy the rest of the application code to the container's working directory.

The `RUN` directive builds the application by running `npm run build` it also  removes development dependencies using `npm prune --production`.

##### Production Stage

The `WORKDIR` directive sets the working directory for the container to /client. Then `COP` directive copies only the necessary files from the build stage to the production stage which include the build directory, public directory, src directory, and package\*.json files.

The `ENV` directive sets the NODE_ENV environment variable inside the Docker image to production.

The `EXPOSE` directive the port 3000 to be exposed by the container when it is running.

The `RUN` directive is used to execute the following commands; `npm prune --production` to remove development dependencies and also clears the npm cache and removes any temporary files using `npm cache clean --force` and `rm -rf /tmp/*` respectively similar to that in build stage.

`CMD` directive specifies the default command to run when a container is started and in this case.

### _Backend_

The backend Dockerfile does not implement multi build as the build scripts are not included in the application

`WORKDIR` directive sets the working directory for the container to /backend.

Then, the `COPY` directive copies the package.json and package-lock.json files to the /backend directory which are required to install the necessary dependencies for the application.

The `RUN` directive is used to execute the following commands; `npm install --only=production` to installs only the production dependencies and also clears the npm cache and removes any temporary files using `npm cache clean --force` and `rm -rf /tmp/*` respectively.

The `COPY` directive is the used to copy the rest of the application code to the container's working directory.

The `ENV` directive sets the NODE_ENV environment variable inside the Docker image to production.

The `EXPOSE` directive the port 5000 to be exposed by the container when it is running.

`RUN` execute the following commands; `npm prune --production` to remove development dependencies and also clears the npm cache and removes any temporary files using `npm cache clean --force` and `rm -rf /tmp/*` respectively similar to that in build stage.

`CMD` directive specifies the default command to run when a container is started .

## 3. Docker-compose Networking

Had both the frontend and Backend Tier network to isolate and also for security puporses as below.
This is to enable the frontend to communicate with backend and backend to communicate to Database using different networks.
```
networks:
  frontend-tier-network:
    ipam:
      driver: default
      config:
        - subnet: 172.128.0.0/16
  backend-tier-network:
    ipam:
      driver: default
      config:
        - subnet: 172.127.0.0/16
```

## 4. Docker-compose volume

A backend_voulme as been defined. The volume is then mounted to the container's /data/db directory using the syntax backend_volume:/data/db in the database service. This will help persist data written

## 5. Git workflow used to achieve the task

Dockerfile are version-controlled using Git alongside other project files, and changes are  to be committed and pushed to a Git repository.
[Docker-Hub](https://github.com/Esthernjoki/yolo.git)
