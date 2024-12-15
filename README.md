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

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/e93e70a6d878247407e3188cd2680f3d9345d1eb/Screenshot%202024-12-14%20103641.png)

## Add items to DynamoDB table
Select your newly created table. On the left pane, click “Explore items”, then “Create item”.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/62ba75d203ac80be644c3f5ddb11a08e8e62d92c/Screenshot%202024-12-14%20103832.png)

You can now begin populating your attributes with the corresponding values . In my example, I added the Player “Sanchez” from the “Goal Keeper” position.

![iamge alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/b6a2f3893c659bcdf6f891f3bdc421ffbac6b40c/Screenshot%202024-12-14%20104015.png)

After adding all items to the DynamoDB table, you can preview the table and all its items, as show below. This MarvelSuperhero table has been populated with 11 Marvel Superheroes from different Movies.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/44a33e6befcf8ab5fb8027bf44960428c3bd29a3/Screenshot%202024-12-14%20104422.png)

Now that we’ve created and populated our DynamoDB table with our items, we can proceed to Step 2: IAM Role!

# Step 2: Launch EC2 with IAM role to scan DynamoDB table

# Create IAM Role to allow read-only access authorization

Navigate to the IAM dashboard and on the left plane click “Roles”, then “Create role”.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/30fa60ae4f25b0729fd72cd2726f5cf29f965766/Screenshot%202024-12-14%20104951.png)

Proceed to select “AWS service”. Our use case will involve using an EC2 Instance to access our DynamoDB table, so we will select “EC2”, then click “Next”.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/7624d2eaafd5252e720ea5b6ff3146cc68b53410/Screenshot%202024-12-14%20105108.png)

Now let’s search through the AWS managed IAM policies. These policies cover common use cases and are provided by AWS in your account. Search “DynamoDB”, select the “DynamoDBReadOnlyAccess” policy, then click “Next”.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/a5463ec34e8f37a673d0b5f52c5567e08ea55742/Screenshot%202024-12-14%20105210.png)

The policy below, written in JSON, “Allows” the assuming of the role “Action” from the EC2 Instance “Service”. Proceed by naming your IAM role, then clicking “Create role”.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/b481adf5be6aaa456f6422734d13c8e2f392de95/Screenshot%202024-12-14%20105618.png)

# Launch EC2 Instance
We need to create an EC2 Instance which we will connect into to use the AWS CLI to scan our DynamoDB table.

Let’s navigate to the EC2 dashboard and click “Launch instance”, as seen below.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/eba4cf962aedc0e03ca8727a6802b466b77b5a29/Screenshot%202024-12-14%20110047.png)

You should now see your Amazon EC2 configuration options page. Note, most of the configurations will remain at their default state to launch our required EC2 Instance.

We will select the “Amazon Linux 2 AMI”, as seen below.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/2267274f3f90702b736639d78613e7646a5e211c/Screenshot%202024-12-14%20110130.png)

Proceed to the Instance Type options —

We will choose the “t2.micro” which is part of the AWS free tier.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/288819921e622eb124c837425025de0094183c13/Screenshot%202024-12-14%20110145.png) 

Proceed to the Key pair option —

Click on the “Create new key pair” to create a new key pair, then enter your desired key pair name. Select “RSA” for key pair type and “.pem” for private key file format.

Click on “Create key pair”, as show below. The “.pm” file should automatically start downloading on your local system. Locate the file after the download is complete and store it in a safe directory. Later, we can use this key pair to connect to our EC2 Instance through ssh.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/615821974c5028f1eb4d284e72ee6d7a4746db3d/Screenshot%202024-12-14%20110310.png)

Continue to the Network settings —

We will use our default VPC. Also, we will keep “Auto-assign public IP” enabled, as this allows our EC2 Instance to automatically receive a public IP address to enable it to connect to the internet upon launch.

We will leave these setting in the default state, as seen below.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/e5e9e39acc64dea1e61de1c10e422c30b274d0f6/Screenshot%202024-12-14%20110344.png)

Continue to the Firewall (Security Group) settings —

We will allow “SSH” traffic to enable us to securely connect to our EC2 Instance and also “HTTPS” and “HTTP” so we can send and received on our browser over the internet.

Note — Allowing from “Anywhere

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/97022b1cca9a5136b114cf0bd803f46a1c98d58b/Screenshot%202024-12-14%20110418.png)

Continue to the “Summary”, then click “Launch instance”.

# Attach IAM Role to EC2 Instance
We now need to attach our DynamoDB read-only access IAM role to our EC2 to allow us to scan our DynamoDB table. Proceed by navigating to your EC2 Instance and selecting it. Click “Actions”, “Security”, then “Modify IAM role”, as shown below.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/08af2017740d67bd4e2040315d1ffd9c087e726d/Screenshot%202024-12-14%20110528.png)

Choose the IAM role previously created, then “Update IAM role”.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/bb29f24fec49870b267bde1ab585dc754d50228b/Screenshot%202024-12-14%20110549.png)

We’ve now launched our EC2 Instance with an IAM role authorizing read-only access to our DynamoDB table. Let’s proceed to Step 3: Scanning the DynamoDB table!

# Step 3: Verifying scanning of our DynamoDB table
Connect into EC2 Instance
Navigate to the EC2 dashboard, then select your EC2 Instance.

EC2 Instance Connect

Connecting using “EC2 Instance Connect”— Select your new EC2 Instance, then click “Connect” on the top right of the pane. Choose the “EC2 Instance Connect” tab, then click “Connect”, as show below.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/51c90743481a7ac5f5ff651de3374937ffbcf665/Screenshot%202024-12-14%20110935.png)

EC2 Instance, your display should be similar to the one below.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/cef3961a29fd6f5fca446d30e056ec6ef1c6cc83/Screenshot%202024-12-14%20111018.png)


We can now verify that we can scan the DynamoDB table, which assures us that we have read access.

To scan your DynamoDB table, run the AWS CLI command below with the name of your DynamoDB table —

"aws dynamodb scan --table-name <name_of_dynamodb_table> --region us-east-1"

If the command ran successfully without any errors, the items in your table should be displayed, as show below.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/86aa83c1384bfd4287382e81f13e588ee5873208/Screenshot%202024-12-14%20111821.png)


# Success!
You were able to scan your DynamoDb table and read the items stored in it! Now we should validate our read-only access permissions in Step 4!

Step 4: Validate inability to write item to DynamoDB table
We shouldn’t be able to write to our DynamoDB table based on our IAM Role which allows read-only access from our EC2 Instance. Let’s test this out by attempting to write a new item to our table.

Run the following command to attempt to write an item to our DynamoDB table —
"aws dynamodb put-item --table-name <table_name> --item '{"<partition_key>": {"S": "<value>"},"<sort_key>": {"S": "<value>"}}' --region us-east-1"

You should receive an “AccessDeniedException” error, stating that we are not “authorized to perform” put-item to write to our DynamoDB table, as shown below.

![image alt](https://github.com/Tatenda-Prince/Creating-DynamoDB-Table-And-Configure-Access-with-IAM/blob/3bc91adcb6e504982c4a9489bbf87df84030c19e/Screenshot%202024-12-14%20113029.png)

# Congratulations!
You’ve just become a “Football Manager!” You’ve successfully created an Amazon DynamoDB table and configured read-only authorization access from EC2 Instances using IAM!











