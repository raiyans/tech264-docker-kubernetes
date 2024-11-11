# Creating an Image from a Container

## diagram 
![alt text](/images/howdocker-works.png)

### Commit the running container to an image:
1. `docker commit <container ID> <dockerhub-username>/<image-name>` creates an image of the running container specified.

![alt text](/images/dockercommit.png)

### Log into Docker Hub:
Ensure you're logged in to your Docker Hub account to push the image. This is required to `push`.

1. `docker login`

![alt text](/images/dockerlogin.png)

### Push the Image to Docker Hub:
1. `docker push <dockerhub-username>/<image-name>`

![alt text](/images/dockerpush.png)

#### How it looks on Docker....
![alt text](/images/nginxcustomimage.png)

### Run the Container to Ensure Success
1. Run the command using `docker run -d -p hostport:80 <dockerhub-username>/<image-name>`. This new image should be based off of the container you made, having all the changes made to the original container. 


# Automate docker image creation using a Dockerfile

1. Use `mkdir <directory name>` to create a new directory in a specifed file path. 
2. `cd` into that folder and use `nano html.index` to create and add text to a HTML page. This is what will appear on your webpage.
3. Use `nano Dockerfile` to create and add some code to a docker file. 

```docker
# Use the nginx base image
FROM nginx:latest

# Copy the custom index.html to the default NGINX web page location
COPY index.html /usr/share/nginx/html/

# Expose 80 to expose the port
EXPOSE 80
```

1. Buld your custom image using `docker build -t <dockerhub-username>/<image-name>:<tag> .`: The `.` inside is intentional and used to specify the directory containing your `Dockerfile` and other files. *Note! The `<tag>` specifies the version of the build.*
2. Run the container using `docker run -d -p hostport:80 <dockerhub-username>/<image-name>:<tag>`, which should then verify if it's been created successfully. *Note! You can delete this after, it was just to test it's working before pushing.*
3. `docker login` to login to your docker, allowing a `push`.
4. `docker push <dockerhub-username>/<image-name>` to push it to the DockerHub account. 
5. `docker run -d -p hostport:80 <dockerhub-username>/<image-name>:<tag>` Run the image custom image command. 

***Blockers to be aware of!!***

1. If you do not put your `<dockerhub-username>` before the image-name, you will be required to use `docker tag`. This command creates a new tag for an existing image, adding a reference to it. Without it, it's only built locally - not on Docker Hub.

# Run Sparta test app in a container

1. Create a new `Dockerfile` to house the code that will provision the app. 

```dockerfile
# Use an official Node.js -v16 image as the base. 
FROM node:20-alpine3.20

# Add a label to specify metadata, such as the purpose of this image.
LABEL description="Node.js app Docker container for test app"

# Set the default working directory to /usr/src/app in the container.
WORKDIR /usr/src/app

# Copy the app folder and package.json files into the container's working directory.
COPY app /usr/src/app
COPY package*.json ./

# Install Node.js dependencies defined in package.json.
RUN npm install

# Expose the port that the app runs on (e.g., 3000).
EXPOSE 3000

# Start the application with `node app.js` or `npm start`.
CMD ["npm", "start"]
```
2. Copy the `app` folder into the folder where you have made your `Dockerfile`.
2. Follow the workflow for `docker build`, `docker run`, `docker login`, `docker push`, and `docker run`. Found [here](#automate-docker-image-creation-using-a-dockerfile). **Voila**!
