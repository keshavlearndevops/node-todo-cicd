# Node Todo Application with CI/CD using Jenkins

The **Node Todo Application with CI/CD using Jenkins** is a comprehensive project designed to showcase the implementation of Continuous Integration and Continuous Deployment (CI/CD) using Jenkins for a practical real-world use case. This project revolves around a dynamic and user-friendly todo application built on the Node.js platform, focusing on enhancing development efficiency and automating deployment processes.

![cicd](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/051b46d5-c165-4542-bf09-1492364c1696)


## Features

- **Todo Management**: Seamlessly manage your tasks with a user-friendly todo application.
- **CI/CD Integration**: Employ Jenkins to orchestrate the entire CI/CD pipeline for automatic building, testing, and deployment.
- **GitHub Webhooks**: Automate the Continuous Integration process by leveraging GitHub Webhooks to trigger builds upon code commits.
- **Dockerization**: Utilize Docker to containerize the application, providing consistent and isolated runtime environments.
- **Infrastructure as Code**: Use the provided Dockerfile and docker-compose.yml to define the application's infrastructure in code.
- **Effortless Setup**: Clone the repository, and the CI/CD pipeline will automatically build and deploy the application using Docker Compose.
- **Working with Agents**: In this Project, we have used the Jenkins Master-Agent Architecture. All the jobs are built by the Agent only.

## Prerequisites

1. Jenkins Master and Agent must be set up, refer to this [blog](https://keshavbathla.hashnode.dev/mastering-jenkins-agents-effortless-two-tier-app-deployment-on-the-freestyle-project).
2. Docker and Docker Compose must be installed (`sudo apt install docker.io -y` and `sudo apt install docker-compose -y`).
3. Port number 8080 must be added as an inbound rule for the Master Instance, and 8000 must be added to the inbound rule of the Agent.
4. Basic Knowledge of `docker-compose.yml` file and Docker.

## Getting Started

To experience the seamless integration of CI/CD using Jenkins and the power of Docker, follow these steps:

1. Fork this repository: Fork the repository as shown below by clocking on the Fork icon on the top right. 
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/fd354b6e-4ecb-486d-a3e0-2f9878276b6e)

2. Starting the Jenkins Master and Agent: Let's start our both Jenkins Master and Jenkins Agent. Log in to the Jenkins UI and as we have stopped our Agent Its Public IP got changed, so we have to update the changed IP in the Agent configuration on the Jenkins Console. 
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/e8fbf6d1-3eba-4524-b1d3-1c83e51945d0)

3. Create a Freestyle Project, I named it *my-node-freestyle* and select the GitHub Project and paste the code URL, I want it to run on my agent only so click on __Restrict where this project can be run__ and type your agent name as shown in the image.
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/9dee920d-1c46-449f-9bdc-bf37b251b65f)

4. Integrating GitHub Webhook:They trigger Jenkins to build, test, and deploy code when GitHub events like pushes and pull requests occur, enabling real-time feedback, automated testing, and efficient development and deployment processes.Go to the GitHub Repository on top of it you will see _settings_, click on it and you will see _webhook_ in the list click on it and hit _Add Webhook_, put the payload URL as `Jenkins-ip/github-webhook/` as shown in the image below, reload the page and it will show a _green_ tick.
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/ee8a09a6-e4c7-457b-aca4-cf644df84df9)

5. Reload the page of github, the webhook will be active as shown by the green tick.
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/a26c3cd4-b8da-4e0b-8353-19db08f6cdb9)

6. Select the GitHub hook trigger for GITScm polling so that our Jenkins must know about the webhook.
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/8b59774a-54b2-4e7c-8d45-f45905e1191d)

7. In the Source Code Management step do the following changes.
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/87582133-5d95-46b6-ae51-75073bef227d)

8. Viewing the Dockerfile and learning what things are there.
 ```Dockerfile
# Use the official Node.js 12.2.0 image based on Alpine Linux as the base image
FROM node:12.2.0-alpine

# Set the working directory inside the container to /app
WORKDIR /app

# Copy all files and directories from the current directory (where the Dockerfile is located)
# to the /app directory inside the container
COPY . .

# Run the npm install command inside the container to install dependencies
RUN npm install

# Run the npm run test command inside the container to execute tests
RUN npm run test

# Expose port 8000 from the container to the host machine
EXPOSE 8000

# Specify the command to run when the container starts
CMD ["node", "app.js"]
```
9. Creating a docker-compse.yml file
```docker-compose.yml
# Specify the version of Docker Compose file format
version: '3.9'

# Define a service named 'web'
services:
  web:
    # Build the Docker image using the Dockerfile in the current directory
    build: .

    # Map port 8000 on the host to port 8000 in the container
    ports:
      - "8000:8000"
```
10. In the Build Steps on the Jenkins console, select execute shell and type in the below commands
```bash
# Stop and remove containers, networks, and volumes defined in the docker-compose.yml file
docker-compose down

# Start the 'web' service in detached mode with options:
# -d: Detached mode, run containers in the background
# --no-deps: Do not start linked services (dependencies)
# --build: Build images before starting containers
docker-compose up -d --no-deps --build web
```
11. Now just do any modification in the code on the GitHub Repository and commit it. I did the following change in _views>todo.ejs_ file as shown below
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/c34bdaef-ecc4-4eb2-b891-0668d58c23a7)

12. This is the screen of Jenkins before you do a commit on the GitHub Repository, there is just 1 build which got created when I changed the docker-compose file on the GitHub
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/9238c2ea-01fe-4e22-8fb1-6c7ca61b6979)

13. Now let's do a commit on the repository and see the Jenkins Magic. As soon as you hit commit you will see a new build is triggered as shown below
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/f84e5f2a-ea8a-432e-aac5-8c0aff4d2a89)

And when you see the Console Output of Build Number 2, it will show that it got triggered by GitHub push event and it is running on my dev agent as shown below
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/2f5ccdd6-28a8-49eb-8dd0-7938a612b9f1)

14. Okay now let's check if it is working and reflecting our modified changes. But first, make sure to add an inbound rule of port 8000 on the agent instance as we are exposing it from Dockerfile and docker-compose file. I have already added it, lets copy the public IP of the Agent Instance and append it with the port 8000 like 54.88.127.94:8000 and hit enter in a new tab. If you got the below output then my friend you have completed the Project. Happy Learning!
![image](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/cac2157c-ce4b-488f-8a27-b2b16f2bf05c)













