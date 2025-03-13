# 🖥 Serverless Sending Application

## 📌 Project Description  
This project demonstrates sending Email and SMS from a website using **Serverless** services from Amazon Web Services (AWS).

---

## 🛡️ Skills Gained
✅ **Create and send** an email with AWS using **SES** and **Lambda**.  
✅ **Create and send** an email with AWS using **SNS** and **Lambda**.  
✅ **Manage** email and SMS sendings using **Step Functions**.  
✅ **Execute** email and SMS sendings from a website using **S3**, **API Gateway**, and **Lambda**.  

---

## 🔧 Tools Used  
| Tool            | Description |
|----------------|------------|
| **Amazon S3** | A cloud storage service that lets users store and retrieve data online. |
| **Amazon API Gateway** | A fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. |
| **AWS Lambda** | A service that lets you run code without managing servers. |
| **AWS Step Functions** | Lets you coordinate individual tasks into a visual workflow. |
| **Simple Notification Service (SNS)** | A web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients. |
| **Simple Email Service (SES)** | A cloud-based email service provider that can integrate into any application for high-volume email automation. |
| **HTML** | Hypertext Markup Language used to build up website's front end. |
| **JS & JSON** | JavaScript and JavaScript Object Notation. |
| **Python** | A versatile programming language primarily used for web development, data analysis, machine learning, scripting, and automation. |

---

## 📋 Table of Contents

1️⃣ [Topology](#topology)

2️⃣ [Create a Lambda Role](#create-a-lambda-role)

3️⃣ [Send Email using SES and Lambda](#send-email-using-ses-and-lambda)

4️⃣ [Send SMS using SNS and Lambda](#send-sms-using-sns-and-lambda)

5️⃣ [Manage how to send using Step Functions](#manage-how-to-send-using-step-functions)

6️⃣ [Create the REST API Handler using Lambda](#create-the-rest-api-handler-using-lambda)

7️⃣ [Create and configure the API Gateway](#create-and-configure-the-api-gateway)

8️⃣ [Code a website and upload it to S3](#code-a-website-and-upload-it-to-s3)

---

## Topology

---

## Create a Lambda Role

Lambda interacts with SES, SNS, Step Functions which means a Lambda Role must be created with executioon rights for these services.

First, go to **IAM**, and select **Roles** on the left panel, then **Create Role**:

<p align="center">
<img width="1200" alt="Create role" src="https://i.gyazo.com/d21aa5631f3fc70d1ae4efd64b5fe33e.png">
</p>

Next, select **AWS service** as the **Trusted entity type** and **Lambda** as the **Use case**:

<p align="center">
<img width="1200" alt="Selected trusted entity" src="https://i.gyazo.com/f6057281e25431bbd1f2298f010169d8.png">
</p>

Continue and a menu permissions will appear:

<p align="center">
<img width="1200" alt="Adding permissions" src="https://i.gyazo.com/84998b3835a5460b2c25ea4629a0cab9.png">
</p>

Now search for the name of each service that will be used, and select *Full Access* rights in order to send instructions and information to the services.

> It is best practice to refine the permissions by using only the rights necessary for the actions you want to do with the services, and to do this you must create our own permissions. Here, to facilitate the execution of the project, the *Full Access* permissions created by default by AWS are used.

Add the *Full Access* permissions of SES:

<p align="center">
<img width="1200" alt="SES Permissions" src="https://i.gyazo.com/add4464f734e5f2bce68547bfe99ced6.png">
</p>

Add the *Full Access* permissions of SNS:

<p align="center">
<img width="1200" alt="SNS Permissions" src="https://i.gyazo.com/33ea90095d45f7cb25239c51cf793539.png">
</p>

Add the *Full Access* permissions of Step Functions:

<p align="center">
<img width="1200" alt="Step functions Permissions" src="https://i.gyazo.com/95c07c8f32f05844fde7633899fe6f31.png">
</p>

Only task left in this section is to validate and create the Lambda Role with the desired name:

<p align="center">
<img width="1200" alt="Lambda Role created" src="https://i.gyazo.com/4fae2e1eee126d9b8f11882f071cd7e0.png">
</p>

Once the Lambda Role has been created, it is time to create the first Lambda function: *email.py*.

---

## Send Email using SES and Lambda

---

## Send SMS using SNS and Lambda

---

## Manage how to send using Step Functions

---

## Create the REST API Handler using Lambda

---

## Create and configure the API Gateway

---

## Code a website and upload it to S3

---
