# Hands on Workshop: Automated Blue/Green K8s Deployment Exercise with JFrog & Nginx

This workshop will walk through an example of a “production ready” Kubernetes deployment with logging, observability, and an example microservice application that is deployed to AWS infrastructure via the JFrog Platform. This will include standing up an AWS VPC cluster, Elastic Kubernetes Service (EKS), and the NGINX Kubernetes Ingress Controller (KIC) using the JFrog Platform for artifact storage and automation.

We will be leveraging the NGINX MARA project, which allows you to easily deploy this infrastructure in your own environment for continued experimentation. You will learn by example some best practice deployment patterns and strategies needed to build a fully automated deployment pipeline, with automated blue/green deployments.

To get the most out of this workshop, you should have a familiarity with Kubernetes, the Linux CLI (or comparable command line utility), Docker, and an understanding of automation tools and software. 

## Instructions

### Step 1: Sign up for required accounts

- #### GitHub Account (Admin access) [Sign up](https://github.com/signup)

During the course of this workshop, you will be need to have the ability to fork accounts and to set up a Personal Access Token to integrate with JFrog Pipelines.

- #### Pulumi Account [Sign up](https://app.pulumi.com/signup)

The first part of this workshop will be standing up an existing application and its infrastructure. Pulumi is a convenient tool used for this project and required by the automated scripts you will run.

Login and create a token here (https://app.pulumi.com/YOUR-PULUMI-USER/settings/tokens) to authenticate from the CLI. Stash this somewhere... you will need it later!

- #### JFrog Platform Account [Sign up](https://jfrog.co/JFrogNGNIX_4959)

This is a free account! In order to utilize Pipelines, you will need to provide a credit card, but you will **NOT** be charged unless you choose to upgrade your account.

!!! When signing up for the JFrog Platform Cloud Free Tier, ensure that you select **AWS** and the **US West 2 (Oregon)** region. !!!

![Screen Shot 2022-03-24 at 5 50 19 AM](https://user-images.githubusercontent.com/116261/159910660-6090b18e-31ad-4b6d-88ac-06f76df2f309.png)

After you receive your credentials by email and on your first login, select the following to ensure you get access to all of the features we will be using:

![Screen Shot 2022-03-24 at 5 58 42 AM](https://user-images.githubusercontent.com/116261/159911604-a455eba7-fdbb-4962-bd3c-384b6ea48a79.png)


- #### AWS Account

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

### Step 3: Update IAM settings for your workspace

1. Return to your Cloud9 workspace and click the gear icon (in top right corner).

2. Select **AWS Settings**.

3. Turn off **AWS managed temporary credentials**.

4. Close the **Preferences** tab.

![image](https://user-images.githubusercontent.com/116261/159907677-143112ab-4c67-4cc4-a004-a7f185b2c648.png)

5. Copy and run the shell commands below in your Cloud9 terminal. These shell commands will:

- Install jq- jq is a command-line tool for parsing JSON

- Ensure temporary credentials aren’t already in place.

- Remove any existing credentials file.

- Set the region to work with our desired region.

- Validate that our IAM role is valid.

```bash
sudo yum -y install jq
rm -vf ${HOME}/.aws/credentials
export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
test -n "$AWS_REGION" && echo AWS_REGION is "$AWS_REGION" || echo AWS_REGION is not set
echo "export ACCOUNT_ID=${ACCOUNT_ID}" | tee -a ~/.bash_profile
echo "export AWS_REGION=${AWS_REGION}" | 
tee -a ~/.bash_profile
aws configure set default.region ${AWS_REGION}
aws configure get default.region
aws sts get-caller-identity --query Arn | grep JFrog-Workshop-Admin -q && echo "IAM role valid" || echo "IAM role NOT valid"
```

If the IAM role is not valid, **DO NOT PROCEED**. Go back and confirm the steps on this page.

![image](https://user-images.githubusercontent.com/116261/159908647-5174a67b-876f-405e-81b4-0ea26834e9ad.png)

### Step 4: Setup IAM user

In your Cloud9 terminal, run the following commands to create a user. 
Make sure to choose a unique username (*YOURUSER*) in the event you are sharing AWS resources.

```bash
aws iam create-user --user-name YOURUSER
```
```bash
aws iam create-access-key --user-name YOURUSER
```
```bash
aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/AdministratorAccess --user-name YOURUSER
```

Copy the output of these commands. You will need this later!

![image](https://user-images.githubusercontent.com/116261/159914169-4dd571fd-53e7-4edf-8ea4-4438e62030c7.png)


