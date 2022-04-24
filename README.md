# Activity 1 - Network diagram

[Lucidchart link](https://lucid.app/lucidchart/c2369a8e-897a-4166-b95e-64de7634eb96/edit?invitationId=inv_dcc7ef59-0b4d-4b97-810a-8be4e0549dcc)

The design consist of:
- A reversed proxy and a load balancer to receive the request and redirect them to the different Frontend's instances.
- Differents instances of Frontend, scaling horizontally depending on the load they receive.
- A load balancer to balance the requests between the Backend.
- The different Backend's instances, scaling horizontally depending on the requests they receive.
- The Backend connects to the NoSQL database; also, it connects to a Redis cache database that connects to the SQL database.

---
---

# Activity 2 - Django and React.js app's deploy

## How to deploy

To deploy correctly, the steps to follow are:
1.  Build the backend image.
2.  Build the frontend image.
3.  Deploy with docker-compose.
4.  *This step make it only if it is necessary.* Allow trafic in the 3000 port in our cloud provider.

<br/>

### Building the backend image

Go to the backend folder and build the image with the name "theback".

```
# From the activity2 folder
cd backend/
docker build -t theback .
```

<br/>

### Building the frontend image

Go to the frontend folder and build the image with the name "thefront".

```
# From the activity2 folder
cd frontend/
docker build -t thefront .
```

<br/>

### Deploy the complete app

From the activity2 folder, execute a docker-compose up command

```
# From the activity2 folder
docker-compose up
```
*You can use the -d option to run it detached.*

<br/>

---

<br/>

### Backend's Dockerfile explained

<br/>

```
FROM python:3.7.13-bullseye # We start from a python/debian image.

WORKDIR /backend # Creating and establishing our working directory.

COPY [".", "."] # We copy all the files from our folder to the image.

RUN apt update # We make an update because we need to install a lib.

RUN apt install -y libgraphviz-dev # We install the lib in order to build the requirements successfully.

RUN pip install --no-cache-dir -r requirements.txt # We install all the dependencies that our app needs.

EXPOSE 8000 # We expose the necessary port

CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"] # We deploy our app
```

---

<br/>

### Frontend's Dockerfile explained

<br/>

```
FROM node:16.14.2-bullseye # We start from a node/debian image.

WORKDIR /frontend # Creating and establishing our working directory.

COPY ["package.json", "package-lock.json", "./"] # We copy our app's dependencies, we do it in order to cache them first (maybe we want to change the app later and the dependencies will no longer need to be installed).

RUN npm install # We install our app's dependencies.

COPY [".", "."] # We copy the rest of the app's files.

EXPOSE 3000 # We expose the necessary port

RUN npm install -g serve # We install "serve" in order to be able to serve the app at the end.

RUN npm run build # We build our React's app.

WORKDIR /assets/bundles # We move our working directory to the built app.

RUN mkdir -p static/public_bundles # We make this folder to be able to serve it.

RUN mv asset-manifest.json favicon.ico js service-worker.js css manifest.json static/public_bundles # We move the files in the correct folder to serve them.

CMD ["serve"] # We serve our app.
```

---

<br/>

### docker-compose explained

<br/>

```
services:
  front: # Our frotnend's service name
    image: thefront # Our image name built in the frontend's Dockerfile.
    ports:
      - "3000:3000" # We expose our machine's port and match it with the container's port.

  back: # Our backend's service name
    image: theback # Our image name built in the backend's Dockerfile.
    environment:
      SECRET_KEY: "${SECRET_KEY}" # In order to use the environment variable, we need to create a .env file with SECRET_KEY=thisIsASecretKey line on it in the root folder.
    expose:
      - "8000" # We expose the container's port to the network.
```
---
---

# Activity 3 - Jenkins' and Docker's deploy

In order to execute the deploy, you need:
-   A Docker's hub account and repository.
-   A Jenkins' job properly configured with your GitHub's repository.
-   Configure GitHub's webhook with Jenkins' server.
-   Credentials of the Docker's hub account in Jenkins.
-   Change the Jenkins file to match your assets.
