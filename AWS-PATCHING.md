# AWS EC2 Instance OS Patching Documentation

## Table of Contents

1. **Introduction**
2. **Prerequisites**
3. **Patch Management Options**
4. **Automated Patching with Amazon Systems Manager (SSM)**
    1. Create an IAM Role
    2.  Create an SSM Document
    3. Create a Maintenance Window
    4. Monitor and Review Patching
5. **Patching with Auto Scaling Groups (ASG)**
   1. Configure a Launch Configuration
   2. Update ASG to Use New Configuration
6. **Manual Patching**
   1. Connect to Your EC2 Instance
   2. Apply OS Patches
7. **Best Practices**
8. **Troubleshooting**
9. **Conclusion**

## 1. Introduction

This documentation outlines the procedures for patching your AWS EC2 instances. Maintaining up-to-date operating systems is crucial for security and optimal performance.

## 2. Prerequisites

Before proceeding with patching, ensure you have the following in place:

- An active AWS account.
- Basic familiarity with AWS services and EC2 instances.
- EC2 instances running supported operating systems.
- Appropriate IAM permissions for the actions described.

## 3. Patch Management Options

Three methods for patching are discussed:

### a. Amazon Systems Manager (SSM)

Automated patching using Amazon SSM.

### b. Patching with Auto Scaling Groups (ASG)

Automatically replace instances in an ASG with patched AMIs.

### c. Manual Patching

The traditional approach of manually connecting to instances and applying patches.

## 4. Automated Patching with Amazon Systems Manager (SSM)

### a. Create an IAM Role

- **Step 1:** Navigate to IAM in the AWS Management Console.
- **Step 2:** Click "Roles" and then "Create Role."
- **Step 3:** Select "AWS service" as the trusted entity and "EC2" as the use case.
- **Step 4:** Attach the "AmazonEC2RoleForSSM" managed policy.
- **Step 5:** Complete the role creation, noting the ARN.

### b. Create an SSM Document

- **Step 1:** In the AWS Management Console, access SSM.
- **Step 2:** Click "Documents" and "Create Document."
- **Step 3:** Specify a name and description.
- **Step 4:** Define the SSM document content for patching, including approved patches.
- **Step 5:** Save the document.

### c. Create a Maintenance Window

- **Step 1:** In the SSM console, go to "Maintenance Windows."
- **Step 2:** Click "Create maintenance window."
- **Step 3:** Provide details such as name, description, schedule, and target instances.
- **Step 4:** Add a "Run Command" task, specifying the SSM document for patching.
- **Step 5:** Configure optional settings like task priority and execution role.
- **Step 6:** Review and create the maintenance window.

### d. Monitor and Review Patching

- **Step 1:** Monitor patching progress in the SSM console.
- **Step 2:** Review logs, outputs, and patching results.
- **Step 3:** Configure SSM notifications for patching status.

## 5. Patching with Auto Scaling Groups (ASG)

### a. Configure a Launch Configuration

- **Step 1:** Access the EC2 service in the AWS Management Console.
- **Step 2:** Go to "Launch Configurations" and create a new launch configuration with a patched AMI.
- **Step 3:** Configure instance details, storage, and security groups.
- **Step 4:** Review and create the launch configuration.

### b. Update ASG to Use New Configuration

- **Step 1:** In the EC2 service, go to "Auto Scaling Groups."
- **Step 2:** Select the ASG to update.
- **Step 3:** Click "Edit" and choose the new launch configuration created in the previous step.
- **Step 4:** Save the changes.

## 6. Manual Patching

### a. Connect to Your EC2 Instance

- **Step 1:** Use SSH for Linux or RDP for Windows.
- **Step 2:** Ensure the security group allows SSH or RDP traffic.
- **Step 3:** Use key pairs for secure access.

### b. Apply OS Patches

- **Step 1:** For Linux, use the appropriate package manager (e.g., `yum`, `apt`) to update packages.
- **Step 2:** For Windows, use Windows Update or download patches from the Microsoft Update Catalog.

## 7. Best Practices

Maintain a secure and efficient patching process:

- Regularly schedule automated patching.
- Ensure backups are in place before patching.
- Test patches in a staging environment if feasible.
- Document patching procedures and policies.

## 8. Troubleshooting

Address common issues during patching:

- Review logs and reports in Amazon SSM.
- Verify instance connectivity and IAM role permissions.

## 9. Conclusion

Patching your AWS EC2 instances is a fundamental task to maintain security and reliability. Choose the method that best suits your needs, and periodically review your patching strategy to ensure your instances are protected with the latest security updates.
