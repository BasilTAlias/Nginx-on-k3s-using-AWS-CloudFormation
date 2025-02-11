# Deploying Nginx on k3s using AWS CloudFormation

This document provides a step-by-step guide to deploying an Nginx web server on a k3s (lightweight Kubernetes) cluster using AWS CloudFormation. The deployment can be done using two methods: manually through the AWS Console or programmatically using the AWS CLI in PowerShell.

## Prerequisites

Before proceeding, ensure you have the following:

- An **AWS account** with permissions to create and manage CloudFormation stacks.
- An existing **EC2 key pair** for SSH access to the instance.
- **AWS CLI** installed and configured with appropriate credentials.
- **PowerShell** installed (for command-line deployment in Method 2).

## Method 1: Deploy via AWS Console

This method provides a user-friendly interface to deploy the CloudFormation stack manually.

1. **Log in** to the [AWS Management Console](https://aws.amazon.com/console/).
2. **Navigate to CloudFormation** under the AWS services menu.
3. Click on **Create Stack** and select **With new resources (standard)**.
4. Under the **Specify template** section, choose **Upload a template file**.
5. Click **Choose file**, locate `nginx-CF.yaml` on your local machine, and upload it.
6. Click **Next**.
7. Enter a **Stack name** (e.g., `nginx`). This name will be used to reference the deployment.
8. Configure stack options as needed (optional) and click **Next**.
9. Review the stack configuration and click **Create Stack** to initiate deployment.
10. The CloudFormation service will now create the required resources. Monitor progress in the **Stack Events** tab.
11. Once the stack reaches the `CREATE_COMPLETE` status, go to the **Outputs** section.
12. Locate the **InstancePublicIp** output, which provides the public IP address of the EC2 instance running Nginx.
13. Open a browser and visit `http://<public-ip>:31111` to access the Nginx web server.

# Screenshorts
---------------

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/1.png)

Create Stack

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/2.png)

Uploading template file

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/3.png)

Providing stack name

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/4.png)

Stack creation in progress 

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/5.png)

Stack creation completed successfully

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/6.png)

Output details

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/7.png)

Instance details

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/8.png)

Security group details

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/9.png)

Accessing web page

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/10.png)

Checking Pods details

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/11.png)

Deleting stack

---

![Alt text of the image](https://github.com/BasilTAlias/Nginx-on-k3s-using-AWS-CloudFormation/blob/main/Images/12.png)

Successfully Deleted Stack

---

## Method 2: Deploy via PowerShell

This method allows for automation using the AWS CLI, making it ideal for scripting and continuous deployment.

1. Open **PowerShell** on your local machine.
2. Navigate to the directory containing the `nginx-CF.yaml` file.
3. Run the following command to create the stack:
   ```powershell
   aws cloudformation create-stack `
     --stack-name nginx `
     --template-body file://nginx-CF.yaml `
     --capabilities CAPABILITY_NAMED_IAM
   ```
   This command instructs AWS CloudFormation to create a new stack named `nginx` using the specified YAML template.

4. Monitor the stack creation process by running:
   ```powershell
   aws cloudformation describe-stacks --stack-name nginx
   ```
   This will display the current status of the stack.

5. Once the stack reaches `CREATE_COMPLETE` status, retrieve the public IP of the EC2 instance by executing:
   ```powershell
   aws cloudformation describe-stacks --stack-name nginx --query "Stacks[0].Outputs[?OutputKey=='InstancePublicIp'].OutputValue" --output text
   ```
6. Copy the public IP from the output and open `http://<public-ip>:31111` in your browser to verify the Nginx deployment.

## Cleanup: Deleting the Deployment

To remove the deployment and associated resources, delete the CloudFormation stack by running:
```powershell
aws cloudformation delete-stack --stack-name nginx
```
This command will automatically delete all resources created by the stack, ensuring a clean removal.

## Conclusion

This guide provides two methods for deploying an Nginx web server on k3s using AWS CloudFormation. The manual AWS Console method is user-friendly and ideal for beginners, while the AWS CLI method enables automation and efficient resource management. Depending on your workflow and preference, you can choose the most suitable deployment method.


