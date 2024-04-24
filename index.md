---
layout: default
---

<!-- **bold**, _italic_, ~~strikethrough~~ or `keyword`. -->
<!-- [Link to another page](./another-page.html). -->



Follow my GitHub Account: [Pushkar Kadam](https://github.com/push-istov)

## Prerequisites

---

- System: Ubuntu 22.04.4 LTS (Jammy Jellyfish)
- Install "Docker" on your machine. For installing Docker, visit: [Official Documentation](https://docs.docker.com/engine/install/ubuntu/)

## Overview:

---

Utilizing Docker containers, my project implements a 3-tier architecture, comprising:

  -  Sample-App Container: This container hosts the application logic.
  -  mongo-express Container: It provides a user-friendly interface for MongoDB.
  -  MongoDB Container: This container houses the database.

The application showcases a simple user profile system set up with:

  1.  index.html: Developed using pure JavaScript and CSS styles.
  2.  Node.js Backend: Utilizing the Express module.
  3.  MongoDB: Employed for data storage.

<div style="text-align: justify">

Enabling independent scaling, ensuring consistent deployment across environments, and simplifying maintenance tasks, this modular setup lays a solid foundation for scalable and efficient application deployment.

</div>

## Step 1. Create a docker network for container isolation.

```js
// Docker Command
docker network create <network-name>
```

![Step-Image1](./assets/images/1.png)

## Step 2. Check if the network is created correctly.

```js
// Docker Command
docker network ls
```

![Step-Image1](./assets/images/2.png)

## Step 3. Create "mongodb" container using the following command

```js
// Docker Command
docker run -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --net mongo-network --name=mongodb -d mongo
```

-The default port for the MongoDB container is 27017 (for more details, please refer to the MongoDB DockerHub repository).

  - The -e flag is used for specifying environment variables.
  - We are launching the container within our "mongo-network."
  - Choose the "mongo" image. If no tag is specified, it defaults to the latest tag.
  - Verify the running container using the "docker ps" command.

![Step-Image1](./assets/images/3.png)

## Step 4. Create "mongo-express" container which is used to manage the mongo database using following command.

```js
// Docker Command
docker run -p 8081:8081 --network=mongo-network --name=mongo-express -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password -e ME_CONFIG_MONGODB_SERVER=mongodb  -d mongo-express
```

  -  The default port for the mongo-express container is 8081 (refer to the mongo-express DockerHub repository for more details).
  -  The -e flag is used for specifying environment variables.
  -  We are deploying the container within our "mongo-network."
  -  Refer to the DockerHub mongo-express repository to obtain all necessary environment variables.
  -  Choose the "mongo-express" image. If no tag is specified, it defaults to the latest tag.
  -  Verify the running container using the "docker ps" command.

![Step-Image1](./assets/images/4.png)

## Step 5. Open http://localhost:8081 and create two databases for our Sample-App.

![Step-Image1](./assets/images/5.png)

## Step 6. Create a "user" collection inside our user-accounts database for our Sample-App which we are going to run soon.

![Step-Image1](./assets/images/6.png)

## Step 7. Now clone the Sample-App on to your system and change the directory to app.

- You can clone the Sample-App from here: [Github Link - Pushkar Kadam](https://github.com/push-istov/3_tier_docker_app).

![Step-Image1](./assets/images/7.png)

## Step 8. Create a Dockerfile for our Sample-App

```js
// Docker Command
From node
WORKDIR /app
COPY . .
ENV MONGO_DB_USERNAME=admin
ENV MONGO_DB_PWD=password
RUN npm install
EXPOSE 3000
CMD ["node", "server.js"]
```

- We are using "node" image.
- Initialize the working directory as "/app" or give any other working dir.
- Give the environment variables i.e., username and passwords for our App to run.
- Open port 3000 as mentioned in our Sample-App.
- At last Run "node server.js" using CMD option.

![Step-Image1](./assets/images/8.png)

## Step 9. Change the admin and password you gave earlier in the "server.js" file.

![Step-Image1](./assets/images/9.png)

## Step 10. Check for the already created Images using following command.

```js
// Docker Command
docker images
```

![Step-Image1](./assets/images/10.png)

## Step 11. Create the image for our Sample-App using the docker build command.

```js
// Docker Command
docker build -t <our-sample-app-name> .
```

- "-t" is for tagging our Sample-App image.
- We have added "." at the end because our dockerfile is in current directory where we are standing right now i.e., inside app folder.

![Step-Image1](./assets/images/11.png)

## Step 12. Confirm the created image using following command.

```js
// Docker Command
docker images
```

![Step-Image1](./assets/images/12.png)

## Step 13. Now create a container out of the Sample-App Image using following command.

```js
// Docker Command
docker run -d -p 3000:3000 --network=mongo-network <sample-app-name>
```

- "-d" is used to spin the container in detached mode.
- Check the running container using "docker ps" command.
- We can see now we have 3 containers running.
  - First: Our Sample-App running.
  - Second: mongo-express container.
  - Third: mongodb container (database).

![Step-Image1](./assets/images/13.png)

## Step 14. Open http://locahost:3000 to see our Sample-App home page.

- Click "Edit Profile" button and update the details. It will save the information on to our database.

![Step-Image1](./assets/images/14.png)

## Step 15. After updating the details.

![Step-Image1](./assets/images/15.png)

## Step 16. Go to our "users" collection to verify our data.

![Step-Image1](./assets/images/16.png)

## Step 17. Click on the "userid" to see more detailed info about that user.

![Step-Image1](./assets/images/17.png)

<!--  -->
<!--  -->

---

<br/>

### **Key Features and Benefits:**

<div style="text-align: justify">

Modularity: Each tier of our architecture is encapsulated within its own Docker container, allowing for independent scaling and management.

</div>

<div style="text-align: justify">

Scalability: With Docker's orchestration tools like Docker Swarm or Kubernetes, scaling our application horizontally becomes seamless and efficient.

</div>

<div style="text-align: justify">

Portability: Docker containers are highly portable, enabling consistent deployment across different environments, from development to production.

</div>

<div style="text-align: justify">

Ease of Management: Docker's containerization simplifies deployment, configuration, and maintenance tasks, reducing overhead and enhancing operational efficiency.

</div>

<div style="text-align: justify">

Flexibility: By decoupling the application logic, database, and user interface into separate tiers, our architecture offers flexibility for future enhancements and modifications.

</div>

<!--  -->
<br/>

### **Conclusion**:

<div style="text-align: justify">

By embracing Docker within a 3-tier architecture, we've established a resilient and scalable foundation for deploying modern applications. Regardless of whether you're crafting a small-scale prototype or orchestrating a large-scale production system, the adoption of containerization streamlines both your development and deployment workflows. This empowers you to channel your efforts towards delivering tangible value to your users.

</div>

<br/>

---

### Name: Pushkar Kadam (21BCP372)

_Written on April 23, 2024_
