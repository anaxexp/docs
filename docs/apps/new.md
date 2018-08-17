# Deploying new application

## Step 1 – choose stack and destination server/cluster

![](http://via.placeholder.com/640x360)​
​
Select one of your connected servers or Demo server by AnaxExp where you want to deploy your applications. 

!!! warning "Demo server"
    All applications deployed to Demo server will be automatically cleaned in 12 hours.

Choose a stack for deployment.

Some stacks may provide optional services (e.g. redis as cache storage) that you can either enable or disable in your stack, and multiple implementation for a service (e.g. Nginx or Apache as an HTTP server). You can always enable/disable a service after the deployment.

## Step 2 – configure your application instance
​
![](http://via.placeholder.com/640x360)​
​
Enter the name of your application and select the instance type (Dev by default). Your application hostname will be automatically generated based on the name, you can optionally adjust it. You can change all these settings later after initial deployment. 

## Step 3 – stack configuration (optional)

Some stacks like Drupal and WordPress provide additional configuration on step 3 such as a code deployment workflow and import.
​