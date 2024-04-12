---
id: docker
title: Docker
sidebar_position: 6
---

# Deploying Vania Using Docker

## Deployment Steps

### 1. Build and Run with Docker Compose

To deploy your application, navigate to the directory containing your `docker-compose.yml` and run:

```bash
docker compose up -d
```

This command builds the images for your application and the MySQL database, then starts the containers in detached mode.

### 2. Verify the Deployment

Check the status of your containers:

```bash
docker ps
```

Ensure that both the `app` and `mysql` containers are up and running without issues.

### 3. Access the Application

The application is now accessible at `http://localhost:8000`. You can also connect to your MySQL database
at `localhost:3306` using the specified credentials.

## Updating Your Application

To update your application, you may need to rebuild the Docker image and restart the containers:

```bash
docker compose up -d --build $Container_Name
```

## Logs and Troubleshooting

For logs or to troubleshoot issues, use:

```bash
docker logs app
```

## Conclusion

Using Docker and Docker Compose simplifies the deployment process, ensuring consistency across different environments.
This setup not only provides a quick way to get your application running but also manages dependencies like databases
smoothly.