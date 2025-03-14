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

The first step is to create a Lambda function capable of sending to SES the instruction to send an email.

When writing an email, there are three fields that must be filled in:
- the sender's email address
- the recipient's email address
- The content of the message

The sender's email address is automaticly filled in, and the two other fields will be filled in from the website hosted on S3.

It is therefore necessary to create a Lambda function capable of retrieving the content of the message and the address of the recipient as an event, and to add them to the instruction sent to SES.

#### Prerequisites before starting

What you should know before using SES is that the service can send emails only from verified email adresses and to verified email addresses. For this project, two addresses will be verified: a source email address and a destination email address.

To do this, go to SES > Identities > **Create Identity**:

<p align="center">
<img width="1200" alt="SES Create Identity" src="https://i.gyazo.com/45ef235fc9cd444cde25eb421380b7a3.png">
</p>

Select **Email address** as **Identity type** and fill in the "email address" field with the address that will serve as the source email address. For this example, the source email address is *aws.ken-source<span>@yopmail.com*:

<p align="center">
<img width="1200" alt="Create Source Email Address" src="https://i.gyazo.com/a36a83f2804ecd738ca23a1a018cdfe2.png">
</p>

Check the mailbox and verify the mail address by clicking the link inside the email you just received from AWS. Once validated, this is what should appear in the AWS console:

<p align="center">
<img width="1200" alt="Source Email Address Verified" src="https://i.gyazo.com/9fea149358ee5051ab98acc6992e9aaf.png">
</p>

Repeat process for the destination email address:

<p align="center">
<img width="1200" alt="Create Destination Email Address" src="https://i.gyazo.com/abfd42bfb309bcaea1c52aad0c8fd24f.png">
</p>

Verify status:

<p align="center">
<img width="1200" alt="Destination Email Address Verified" src="https://i.gyazo.com/4ac080d79f6dc460bdad79f8369a849e.png">
</p>

Now time to create the Lambda function *email.py*.

#### Create Lambda function *email.py*

Go to Lambda to create the first function.

The function is called *email* and uses the **Python 3.9** language. In the **Permissions** tab, choose the newly created Lambda role. You can then create the function:

<p align="center">
<img width="1200" alt="Lambda Create Function" src="https://i.gyazo.com/9ad3bce09bcbff36651df047116fcfad.png">
</p>

Once the function is created, go to the **Code source** menu. This can be seen:

<p align="center">
<img width="1200" alt="Code Source" src="https://i.gyazo.com/5502f6cb2735ad30bcceda8ad97fc29f.png">
</p>

Delete the code contained in *lambda_function*. Everything is ready, it's time to code.

In order to interact with AWS services in Python, you need to know the Software Development Kit (SDK) used by AWS. An SDK is a toolkit that a platform provides to developers to interact with its services.

> For those who struggle to understand what an SDK is, I recommend you watch this great video from IBM explaining it: https://www.youtube.com/watch?v=kG-fLp9BTRo

Here, AWS provides its own SDK for python3 named boto3. More details on this link: https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html

In order to interact with AWS services, you must first import the boto3 library inside your code:

``` py
import boto3
```

Here, the service to be interacted with is SES. To do this, you have to call the *client('ses')* class of the boto3 library. This class will be stored in a *ses* variable to simplify the code:

``` py
ses = boto3.client('ses')
```

The library is imported, and the *ses* variable is ready, you now have to work on the *lambda_handler* function. The *lambda_handler* function is the default function in Lambda. It is inside this function that the events received are managed. It is declared in this way:

``` py
def lambda_handler(event, context):
```

The function contains two arguments: *event* and *context*.

*Event* is the event value received by Lambda (the Input), and *context* contains information specific to the function (execution time, name, version etc.). 
> The context argument is of no use here, but for the more curious who want to learn more about the context argument, go to this link: https://docs.aws.amazon.com/lambda/latest/dg/python-context.html

Within this function, the SES appropriate function for sending an email must be filled in. To find the function, let's have a look at the boto3 documentation for SES: https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ses.html

After a little research, the function that fits the bill seems to be *send_email()*:

<p align="center">
<img width="1200" alt="Function Send email" src="https://i.gyazo.com/7a70329ed27c19b6600e3b94a64fb697.png">
</p>

And according to the documentation, three fields are mandatory: **Source**, **Destination** and **Message**. Perfect, the source and destination addresses are ready, and the message will contain what you want.

The *send_email* function to be added to *lambda_handler* should look like this:

``` py
    ses.send_email(
        Source='aws.ken-source@yopmail.com',
        Destination={
            'ToAddresses': [
                'aws.ken-destination@yopmail.com',
            ]
        },
        Message={
            'Subject': {
                'Data': 'Dream Big Work Hard AWS'
            },
            'Body': {
                'Text': {
                    'Data': 'This is my message'
                }
            }
        }
    )
```

:warning: `Be careful, here you do not want to insert a hard-coded destination e-mail address and a hard-coded message, but ONLY a hard-coded source address. The destination address and the message come from the website, so these values will arrive in the event of the Lambda function (remember, the Input)!`

To do this, add two variables "destinationEmail" and "message" from the event, in this way:

``` py
    ses.send_email(
        Source='aws.ken-source@yopmail.com',
        Destination={
            'ToAddresses': [
                event['destinationEmail'],
            ]
        },
        Message={
            'Subject': {
                'Data': 'Dream Big Work Hard AWS'
            },
            'Body': {
                'Text': {
                    'Data': event['message']
                }
            }
        }
    )
```

If you look closely, the destination email address and message have been replaced by **event['destinationEmail']** and **event['message']**. This way, the values of the destination email address and the message will be those retrieved from the event.

All that remains is to add a **return** at the end of the function. It is important to know that the return is received by the one who called the Lambda function. According to the project diagram, it is Step Functions that calls this Lambda function, and Step Functions needs to receive a return in order to operate, but the value of the return does not matter.

Fill in the return at the end of the function, with the value of your choice (in this case the value is *"Email sent!"*):

``` py
    return 'Email sent!'
```

the Lambda function should look like this (with your own source email address):

``` py
import boto3

ses = boto3.client('ses')

def lambda_handler(event, context):
    ses.send_email(
        Source='aws.ken-source@yopmail.com',
        Destination={
            'ToAddresses': [
                event['destinationEmail'],
            ]
        },
        Message={
            'Subject': {
                'Data': 'Dream Big Work Hard AWS'
            },
            'Body': {
                'Text': {
                    'Data': event['message']
                }
            }
        }
    )
    return 'Email sent!'
```

It is now time to Deploy and Test the code:

<p align="center">
<img width="1200" alt="Email Function Finished" src="https://i.gyazo.com/9f3647122185bccdf202ec19d9716c86.png">
</p>

The last step is to configure a test event to simulate the reception of an event. To do this, fill in the **destinationEmail** and **message** variables with the desired values. In this example, **destinationEmail** has the value *aws.ken-destination<span>@yopmail.com* and **message** has the value *Time to test*:

<p align="center">
<img width="1200" alt="Configure Test Event" src="https://i.gyazo.com/04c49c1b359cf2c90713862213b73e48.png">
</p>

All that remains is to press the **Test** button, and there should be a *Succeeded* status displayed in the execution results:

<p align="center">
<img width="1200" alt="Execution Results" src="https://i.gyazo.com/512f4400b7dd794cd35500b4430d8973.png">
</p>

Let's take a look at the destination Mailbox:

![Email received](https://i.gyazo.com/1ac4f5c1d667ed2a29bb8d77b67b2c8f.png)

The message was received by destination address with the correct message!

The same must now be done for the *sms.py* function.

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
