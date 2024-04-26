---
sidebar_label: 'Render'
sidebar_position: 6
---

# Deploying Vania on Render

## Prerequisites

Before you begin, make sure you have the following:

- A Render account
- Your Vania application code

## Deployment Steps

### 1. Create a New Web Service

1. Log in to your Render account and navigate to your [dashborad](https://dashboard.render.com).
2. Click on **New** > **Web Service**.
3. Choose **Build and deploy from a Git repository** then choose your GitHub repository where your Vania application is hosted.
4. Now you are on the **Deploy configuration** page.  Render will auto-detect the Docker runtime environnement.

### 2. Configure Environment Variables

Before deploying the app, you have to set the env variables in the Render dashboard:

1. On the same page, scroll down to **Environment Variables**.
2. Click on **Add from .env** and paste the content of your `vania-project/.env` file.

### 3. Deploy Your Service

1. Once you've configured your service settings, click **Create Web Service**.
2. Render will begin building and deploying your application.
3. Once deployment is complete, your Vania application will be live at the provided URL.

## Updating Your Application

To update your application on Render:

1. Push your changes to your GitHub repository.
2. Render will automatically detect the changes and redeploy your application.
3. You can also manually trigger a redeployment by clicking on the **Deploy** button in your Render dashboard or updating env variables.

## Logs and Troubleshooting

Render provides detailed logs and metrics to help you troubleshoot any issues:

1. In your Render dashboard, navigate to your Vania service.
2. Go to the **Logs** tab to view logs for your application.
3. Use the logs to diagnose and troubleshoot any errors.

## Conclusion

 With Render's seamless integration with GitHub and robust deployment process, you can focus on building your application while Render handles the deployment and hosting.
