# Introduction

Welcome to the Exponential Learning Working Group Co-op repository. Here you will find the underlining database, API controller, and the [website](3.215.148.52). 

# Getting Started

Throughout this project you will find several different technologies such as [Docker](https://docs.docker.com/) (containerization), [Node](https://nodejs.org/en/docs/), and [Yarn](https://classic.yarnpkg.com/lang/en/docs/install/) (package manager). We will not be going through the setup of each resource, there are exponential resources and documentation available to aid in setting up your working environment.

Within the [GitHub Organization](https://github.com/UMD-ELW-Group-Campus-Fabric-Community), there are two working repositories ([Webiste](https://github.com/UMD-ELW-Group-Campus-Fabric-Community/Website) and [Database](https://github.com/UMD-ELW-Group-Campus-Fabric-Community/Database-1)) used as the median for developing code locally and pushing to the EC2 instance hosted by the iSchool. In order to gain access to the instance, you will need to contact the iSchool's Information Technology Team. 

Let's get started!

1. To check that all the resources are running correctly on your machine, you can run the following commands.
```bash
# Docker
docker --version

# Node 
node --version

# Yarn
yarn --version
```
# Folder Structure
Although NextJS is a fullstack development framework, the project was separated into individual pieces to avoid overlapping work. This also allows future teams to redesign individual segmenets of the application with minimal to no additional work needed throughout the rest of the service.

The backend of the application splits the configuration of environment (e.g., defining the Postgres db, configuring environment variables, etc.) and the integration of the API ontop of the database. Refer to the [Database](https://github.com/UMD-ELW-Group-Campus-Fabric-Community/Database-1) repository for more information.

The frontend of the application takes a module approach to developing a website. Pages are defined as templates which select from components, panels, and other elements in order to display the information. This is consistent to the styles section. Changes to individual styles or colors under the `colors.ts` file will be reflected across the entire website. Refer to the [Webiste](https://github.com/UMD-ELW-Group-Campus-Fabric-Community/Website) respoitory for more information.

# Docker and Postgres

The backend of the website utilizes isolated processes in order to restrict direct access to the underlining database. This is used not only to make sure only people who need access to the data requested are granted access, but also to avoid issues when querying data (e.g., wrong parameters, data validation, etc.).

To see the code and documentation for the backend of the application, refer to the [local repository](https://github.com/UMD-ELW-Group-Campus-Fabric-Community/Database-1). 

# Deployment

There are a few steps to deploying the application.

1. In order to connect to the instance, you will need the Privacy Enhanced Mail (PEM) file to validate your SSH connection. To obtain this you will need to go through the iSchool's Information Technology team. Once you have the certificate, you can run the following command to connect to the AWS EC2 instance.

    ```bash
    ssh -i [path to pem] ubuntu@[address to instance]

    # Example 
    ssh -i docker-students.pem ubuntu@1.234.567.89X
    ```

2. Stopping the existing services on the instance. After ssh'ing into the instance, you will need to run the following commands in order to free up the processes running.

    Docker will require you to run the following commands
    ```bash
    # CD to the Database directory
    cd /path/to/database
    # Close any existing databases
    docker-compose down
    ```
    **Note**: Running docker-compose down will get rid of any volumes (i.e., data) that exist in the database. Consider dumping or backing up the data prior to removing the Docker container.

    The NextJS website will require you to run the following commands.
    ```bash
    # CD to the Website directory
    cd /path/to/website
    # Close any active sessions
    sudo screen -r <pid> && :quit
    ```
    You can check if the session was removed by using the following commands 
    ```bash
    # Check port 80
    sudo lsof -i :80
    # Kill process on port if running
    sudo kill <pid>
    ```

3. Update the code in each directory using `git pull`. This will git (pun intended) the code from each repository and write over any existing files.

4. Starting the services back.

    In order to start the docker container back up, you will need to use the following command. The docker containers will run in the background listening to the local ports identified in the `configuration` file. Visit the [database](https://github.com/UMD-ELW-Group-Campus-Fabric-Community/Database-1) repository to learn more.
    ```bash
    # CD to Database directory
    cd /path/to/databse
    # Compose the docker images and run detached
    docker-compose up --build -d 
    ```
    Note: When deploying the application, it is important to note that the only processes within a docker images is the *API* and *Database* and **not** the website.  

    In order to run the website, we will need the following commands. The database will need to be running prior to building and deploying the website.

    ```bash
    # CD to Website directory
    cd /path/to/website
    # Build the website
    sudo yarn build 
    # Starting the production server
    sudo screen yarn start
    ```
    **Note**: Running the `yarn start` command using screen ensures the webserver will continue to run in the event we disconnect.

# Handling Errors

Although there are multiple sources were errors can occur, I want to focus on a few of the more common sources. 

When running `docker-compose`, you may encounter an issue along the lines of "not enough space." Docker does not clean up any loose branches and will need some pruning. Running the following command will remove all images, volumes, containers.

```bash
# Removing everything
sudo docker system prune
# Remove dangling images only
sudo docker image prune
```

When running `yarn start`, you may encounter an issue of "address already in use." You will need to follow [Step 2](#deployment) under Deployment.

