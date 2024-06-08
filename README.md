# Optimize Utilization

## Activity Overview

In this activity, I optimized the AWS resources used to run the Café web application. Specifically, I:

- Uninstalled the decommissioned local database from the Café instance to decrease the instance’s storage requirements.
- Changed the instance type to T3 micro to reduce costs.

This diagram illustrates the topology of the Café web application runtime environment before and after the optimization.

![optimize-resources-architecture](https://github.com/Mohamed-kittany/Canvas-Lab-189-OptimizeUtilization/assets/161580792/68e79487-0ef1-4b1e-8d82-5d2e977a6d9e)

## Activity Objectives

After completing this activity, I was able to:

- Optimize an Amazon Elastic Compute Cloud (Amazon EC2) instance to reduce costs.
- Use the AWS Pricing Calculator to estimate AWS service costs.

## Tasks Completed

### Task 1: Optimize the Website to Reduce Costs

- **Connect to the Café Instance by Using SSH**
  - Connected to the Café instance using SSH on Windows.
  - Configured the AWS CLI environment on the Café instance.
  
- **Connect to the CLI Host Instance by Using SSH**
  - Opened an SSH session to the CLI Host instance and configured the AWS CLI client software on the CLI Host EC2 instance.

- **Uninstall MariaDB and Resize the Instance**
  - Stopped and uninstalled the local MariaDB database from the Café instance using the following commands:
    ```sh
    sudo systemctl stop mariadb
    sudo yum -y remove mariadb-server
    ```
  - Determined the Instance ID of the Café instance:
    ```sh
    aws ec2 describe-instances --filters "Name=tag:Name,Values=CafeInstance" --query "Reservations[*].Instances[*].InstanceId"
    ```
  - Stopped the Café instance and changed its instance type to `t3.micro`:
    ```sh
    aws ec2 stop-instances --instance-ids <CafeInstance Instance ID>
    aws ec2 modify-instance-attribute --instance-id <CafeInstance Instance ID> --instance-type "{\"Value\": \"t3.micro\"}"
    aws ec2 start-instances --instance-ids <CafeInstance Instance ID>
    ```
  - Verified that the Café instance is running and recorded the new Public DNS Name and Public IP Address:
    ```sh
    aws ec2 describe-instances --instance-ids <CafeInstance Instance ID> --query "Reservations[*].Instances[*].[InstanceType,PublicDnsName,PublicIpAddress,State.Name]"
    ```
  - Tested the Café website to ensure it is functional:
    - In a browser window, entered the URL: `http://<Downsized CafeInstance Public DNS Name>/cafe`

### Task 2: Use the AWS Pricing Calculator to Estimate AWS Service Costs

- **Calculate the Costs Before Optimization**
  - Used the AWS Pricing Calculator to estimate the costs of running the website on a T3 small instance with a decommissioned local database.

- **Calculate the Costs After Optimization**
  - Modified the EC2 instance type to `t3.micro` and reduced storage to 20 GB.

- **Estimate the Projected Cost Savings for Café**
  - Calculated the overall projected cost savings by comparing the costs before and after optimization.

## What I Have Achieved/Learned from this Lab

- Learned how to connect to EC2 instances using SSH.
- Configured AWS CLI environment on EC2 instances.
- Uninstalled unnecessary software to optimize resource utilization.
- Modified the instance type to reduce costs.
- Used the AWS Pricing Calculator to estimate and compare costs before and after optimization.
- Achieved a good amount of cost savings in AWS service costs by optimizing resources.

  
