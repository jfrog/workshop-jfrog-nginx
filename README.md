# Hands on Workshop: Automated Blue/Green K8s Deployment Exercise with JFrog & Nginx

This workshop will walk through an example of a “production ready” Kubernetes deployment with logging, observability, and an example microservice application that is deployed to AWS infrastructure via the JFrog Platform. This will include standing up an AWS VPC cluster, Elastic Kubernetes Service (EKS), and the NGINX Kubernetes Ingress Controller (KIC) using the JFrog Platform for artifact storage and automation.

We will be leveraging the NGINX MARA project, which allows you to easily deploy this infrastructure in your own environment for continued experimentation. You will learn by example some best practice deployment patterns and strategies needed to build a fully automated deployment pipeline, with automated blue/green deployments.

To get the most out of this workshop, you should have a familiarity with Kubernetes, the Linux CLI (or comparable command line utility), Docker, and an understanding of automation tools and software. 

## Instructions

### Step 1: Sign up for required accounts

#### GitHub Account (Admin access) [Sign up](https://github.com/signup)

During the course of this workshop, you will be need to have the ability to fork accounts and to set up a Personal Access Token to integrate with JFrog Pipelines.

#### Pulumi Account [Sign up](https://app.pulumi.com/signup)

The first part of this workshop will be standing up an existing application and its infrastructure. Pulumi is a convenient tool used in the project.

#### AWS Account

The project infrastructure for this workshop will be in AWS.

If you have a personal account you would like to use, feel free to do so. Be aware that charges to your account may result during the workshop and afterward if your resources aren't cleaned up.

If you are attending a live workshop event, you may have access to the AWS Event Engine with temporary resources allocated to you during the workshop period and 4 hours after.

##### AWS EVENT ENGINE INSTRUCTIONS
###### Navigate to the following: https://dashboard.eventengine.run/

![image](https://user-images.githubusercontent.com/116261/159903318-58b732a4-8a41-4f5d-907a-c43a728bdbfe.png)

1. Enter the provided hash code in the text box.

2. Click on the Accept Terms & Login button.

3. Select AWS Console.

4. Then select Open AWS Console.
