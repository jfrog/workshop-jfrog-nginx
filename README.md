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

### Step 2: Set up Cloud9 IDE
For a live workshop, it's convenient to work in a consistent environment. If you are confident you can resolve all needed dependencies on your own machine, this step is not required.
[AWS Cloud9](https://aws.amazon.com/cloud9/) is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. Cloud9 comes prepackaged with essential tools for popular programming languages, including JavaScript, Python, PHP, and more, so you don’t need to install files or configure your development machine to start new projects.

##### CLOUD9 SETUP INSTRUCTIONS
1. Within the AWS console, use the region drop list to select us-west-2 (Oregon). The workshop script will provision the resources in this region.

![image](https://user-images.githubusercontent.com/116261/159904564-cbf3570d-105a-4858-aaf0-ef7a0448336f.png)

2. Navigate to the [Cloud9](https://console.aws.amazon.com/cloud9/home) console or just search for it under the AWS services search.

![image](https://user-images.githubusercontent.com/116261/159904718-45ae9fbb-e01d-4cd1-b118-9b4386aa5f02.png)

3. Click the **Create environment** button.

![image](https://user-images.githubusercontent.com/116261/159905085-39923dd3-a409-40f9-88f1-edca00bdde7a.png)

4. For the name, enter *jfrog-workshop* and click Next Step.

5. Select **Other instance type** and choose **t3.medium**.

6. Leave all the other settings as default.

![image](https://user-images.githubusercontent.com/116261/159905607-3698401c-6454-4912-ae7c-98965b428716.png)

7. Click **Next Step**.

8. On the **Review** page, click **Create environment**. The Cloud9 environment will take a few minutes to provision.

9. When the environment comes up, close the **Welcome** page tab.

10. Close the lower work area tab.

11. Open a new terminal tab in the main work area.

![image](https://user-images.githubusercontent.com/116261/159905986-b7a34c10-e99a-433f-857e-408988371c7b.png)

Your workspace should now look like this and can hide the left hand environment explorer by clicking on the left side environment tab.

![image](https://user-images.githubusercontent.com/116261/159906095-6cf74ee0-a8a3-4b5c-a1f5-cc20cdb01735.png)

Your workspace should now look like this and can hide the left hand environment explorer by clicking on the left side environment tab.

If you don’t like this dark theme, you can change it from the **View ► Themes** menu.

Cloud9 requires third-party-cookies. You can whitelist the specific domains. You are having issues with this, Ad blockers, javascript disablers, and tracking blockers should be disabled for the Cloud9 domain, or connecting to the workspace might can be impacted.

