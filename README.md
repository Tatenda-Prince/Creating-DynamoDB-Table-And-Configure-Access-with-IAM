# Creating-DynamoDB-Table-And-Configure-Access-with-IAM

# Intro
In this project, I will show you how to be a Football Manager, by going through the steps to create a "Chelsea FC starting line up" Amazon Dynamo DB table, populate the table with Chelsea's best players and use AWS IAM to demonstrate the principles of least privilege by allowing read-only access to our table using AWS CLI from EC2 Instance. 

# Background
## Amazon DynamoDB (DynamoDB)
DynamoDB is an AWS managed database service that provides fast and flexible storage for applications that need to store and retrieve data quickly. It is a NoSQL database, meaning it does not use the traditional SQL relational database structure. Instead, it stores data in tables with a simple key-value structure.

DynamoDB is designed to handle large amounts of data and support high levels of traffic. It automatically scales up or down to meet the needs of the application.

One of the main benefits of DynamoDB is its low latency. It is optimized for fast reads and writes, making it a good choice for applications that require real-time data access.

## AWS Identity and Access Management (IAM) Role
In IAM, a IAM role is not a user or group, and does not have its own set of credentials. It is an AWS identity with a set of permissions that you can use to grant AWS services and applications access to your resources.

## Partition Key

In Amazon DynamoDB, a partition key is a special type of primary key used to distribute data evenly across the servers in a DynamoDB table, allowing the table to scale horizontally and handle a large amount of data and traffic.

Each item in a DynamoDB table has a unique primary key, which consists of a partition key and an optional sort key, which is used to sort the data within a partition.

## Use Case
I love watching Football and I follow the English premier league games religiously and i support Chelsea Football club. I decided to create a Amazon DynamoDB tanle to store my favorite players and their position on the field and give access to others to view the items in the table.

# Step 1: Create and populate new DynamoDB table
Create DynamoDB table
Navigate to the DynamoDB dashboard and click “Create table”, as show below.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/d6689f3fa3eacd420825ec55506edd3f505d35ba/Screenshot%202024-12-14%20123600.png)

Proceed to name your DynamoDB table, then provide a Partition key and optional Sort key. Note, both keys create a composite primary key, composed of the two attributes. All data under a Partition key is sorted by the Sort key value.

For this demonstration, I have chosen “Player Position” as my Partition key and “Players” as my Sort key because those are my favorite players at Chelsea.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/f36bb8472a51dba9fc46a719df8147fc8142996a/Screenshot%202024-12-14%20103559.png)

After providing a Partition key and Sort key, continue to “Create table”.

Your table should now be created, however, you may need to wait a few minutes until the status is “Active”, as shown below.

![image alt]()

