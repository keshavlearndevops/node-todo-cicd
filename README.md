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

### 1. Fork the Repository

Kickstart your journey by forking this repository. Click on the Fork icon ![Fork Icon](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/fd354b6e-4ecb-486d-a3e0-2f9878276b6e) at the top-right corner to make the repository yours.

### 2. Launch Jenkins Master and Agent

Fire up the Jenkins Master and Agent. Access the Jenkins UI and ensure the Agent's IP is updated in the Jenkins Console if needed.
![Starting Jenkins](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/e8fbf6d1-3eba-4524-b1d3-1c83e51945d0)

### 3. Create a Freestyle Project

Craft your destiny by creating a Freestyle Project named *my-node-freestyle* on Jenkins. Configure it to run on your desired agent, and watch as the path to greatness unfolds.
![Creating Freestyle Project](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/9dee920d-1c46-449f-9bdc-bf37b251b65f)

### 4. Integrate GitHub Webhook

Unleash the power of GitHub Webhooks! These triggers orchestrate Jenkins to build, test, and deploy code. Set up the webhook, add the payload URL, and witness the green tick of validation.
![GitHub Webhook Setup](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/ee8a09a6-e4c7-457b-aca4-cf644df84df9)

### 5. Awaken the Webhook

Experience the awakening of the webhook as the green tick emerges. The realm of automation is at your command!
![Webhook Activation](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/a26c3cd4-b8da-4e0b-8353-19db08f6cdb9)

### 6. Journey with GitHub Hook Trigger

Engage the GitHub hook trigger to connect Jenkins with your webhook. Unleash the true power of automation and unlock the GitHub realm's secrets.
![GitHub Hook Trigger](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/8b59774a-54b2-4e7c-8d45-f45905e1191d)

### 7. Source Code Management Magic

In the realm of Source Code Management, enact the changes that shape destinies. Tune your frequencies to the code's rhythm, and let the enchantment begin.
![Source Code Management](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/87582133-5d95-46b6-ae51-75073bef227d)

### 8. Unveil the Dockerfile Secrets

Unearth the magic within the Dockerfile. Witness the incantations that install dependencies, execute tests, and expose the power of your creation.
```Dockerfile
# Witness the majestic Dockerfile

# Base image: The legendary Node.js 12.2.0 on Alpine Linux
FROM node:12.2.0-alpine

# Set the container's heart at /app
WORKDIR /app

# Infuse the essence of your realm into the container
COPY . .

# Instill the powers of the universe – npm install
RUN npm install

# Let the tests weave their spells
RUN npm run test

# Open the gates to port 8000
EXPOSE 8000

# As the stars align, summon 'node app.js'
CMD ["node", "app.js"]
```
### 9. Embrace Docker-Compose Chronicles

Enter the realm of docker-compose.yml, where you wield the ancient syntax to shape your destiny. Define services, map ports, and orchestrate the dance of containers.

```yaml
Copy code
# Embrace the Docker-Compose Chronicles

# Enchantment level: Docker Compose format version 3.9
version: '3.9'

# Behold the service named 'web'
services:
  web:
    # Forge an image from the Dockerfile's essence
    build: .

    # Map the mystical port 8000
```
### 10. In the Build Steps on the Jenkins console, select execute shell and type in the below commands

```bash
# Stop and remove containers, networks, and volumes defined in the docker-compose.yml file
docker-compose down

# Start the 'web' service in detached mode with options:
# -d: Detached mode, run containers in the background
# --no-deps: Do not start linked services (dependencies)
# --build: Build images before starting containers
docker-compose up -d --no-deps --build web
```
### 11. Code Modification

Start by making a change to the code in the GitHub repository. For instance, I made a tweak to the `_views>todo.ejs` file, as shown below:

![Code Modification](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/c34bdaef-ecc4-4eb2-b891-0668d58c23a7)

### 12. Jenkins Build Process - Pre-Commit Build

Before committing your changes to the GitHub repository, take a look at Jenkins. You'll notice a single build, which was triggered when I modified the `docker-compose` file:

![Pre-Commit Build](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/9238c2ea-01fe-4e22-8fb1-6c7ca61b6979)

### 13. Witness the Jenkins Magic

Now, commit your changes to the repository and experience the magic orchestrated by Jenkins. As soon as you hit the commit button, a new build is set in motion, as illustrated below:

![Jenkins Magic](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/f84e5f2a-ea8a-432e-aac5-8c0aff4d2a89)

### 14. Dive into Console Output Insights

Delve into the details by exploring the Console Output for Build Number 2. This build was triggered by a GitHub push event and is diligently executed by our vigilant dev agent:

![Console Output](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/2f5ccdd6-28a8-49eb-8dd0-7938a612b9f1)

## Final Test and Completion

It's time to put your creation to the ultimate test:

### 15. Project Validation

Before proceeding, ensure that you've added an inbound rule for port 8000 on the agent instance. Since this port is exposed in both the Dockerfile and docker-compose, this step is essential. I've got you covered! Now, with the public IP of the Agent Instance in hand, combine it with port 8000 (e.g., `54.88.127.94:8000`) and enter it in a new browser tab. Witness the output below – if you see it, congratulations! You've successfully completed the Project. Happy Learning!

![Final Test](https://github.com/keshavlearndevops/node-todo-cicd/assets/134159375/cac2157c-ce4b-488f-8a27-b2b16f2bf05c)
