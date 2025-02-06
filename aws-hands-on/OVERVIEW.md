
# AWS Hands-On

Overview of learning objectives:

* Day 1: S3
* Day 2: IAM and CLI
* Day 3: EC2 and Web Servers
* Day 4: IAM Roles and EC2 + S3

## Day 1: S3

### Introduction to S3

Goals: Sign up for AWS, create an S3 bucket, and upload a file to the bucket. Then deploy it as a static website.

* [Create a static site with S3](https://youtu.be/-l83oqcaTHg?feature=shared)

## Day 2: IAM and CLI

### Introduction to IAM and CLI

Goals: Create several IAM users. Create credentials for each user. Use the CLI to interact with S3.

* [Create an IAM User](https://youtu.be/ubrE4xq9_9c)
* [Setup AWS CLI on your computer](https://www.youtube.com/watch?v=vZXpmgAs91s)

Activities:

1. Create an IAM user with programmatic and console access with an S3 policy.
2. Obtain the access key and secret key for the user.
3. Install the AWS CLI.
4. Configure the CLI with the access key and secret key AWS Configure.
5. Use the CLI to interact with S3.

Once you have your CLI set up, you can use it to interact with S3. For example, you can list the contents of a bucket, upload a file, or download a file.

### Challenges:

#### Upload many files to an S3 bucket using the CLI.
  1. Upload a file to S3 using the CLI.
  2. Create a "folder" in S3 using the CLI.
  3. Upload all the files in a folder on your hard drive to S3 using the CLI.
  4. Try uploading all files ending in *.jpg to an S3 "folder" using the CLI.
  5. Try uploading all files ending in *.png to an S3 "folder" using the CLI.
  6. Try deleting all files in an S3 "folder" using the CLI.

Hint: To upload a file to S3, you can use the `cp` command. For example:

```bash
aws s3 cp ./my-file.txt s3://my-bucket/my-file.txt
```

To upload all files ina folder to S3, you can use the `sync` command. For example:

```bash
aws s3 sync ./my-folder s3://my-bucket/my-folder
```

To upload all items in a folder that end in `.jpg` to S3, you can use the `sync` command. For example:

```bash
aws s3 sync ./my-folder s3://my-bucket/my-folder --exclude "*" --include "*.jpg"
```

#### Create several more users with different permissions.
  1. Create a user with read-only access to an S3 bucket. (Don't give console access.)
  2. Create a user with read-write access to an S3 bucket. (Don't give console access.)
  3. Create a user with full access to an S3 bucket. (Don't give console access.)
  4. Create a user with full access to all S3 buckets. (Don't give console access.)
  5. Create a user with full access to all AWS services. (Give console access. Use this to login in the future instead of your root account.)

3. Deploy one of your projects to an S3 bucket as a static website. (Try with a simple HTML and CSS site first.)
  1. Use the CLI to upload your project from your local hard drive to an S3 bucket.
  2. Use the same command and put it in a script in your `package.json` file. (Call it "deploy" or something similar.)

Hint: To upload all items in a folder to S3, you can use the `sync` command. For example:

```bash
aws s3 sync ./my-folder s3://my-bucket/
```


## Day 3: EC2 and Web Servers

* [About EC2 Instances](https://www.youtube.com/watch?v=eo0sp1xzYCY)
* [Create and EC2 Instance and access it with SSH](https://youtu.be/cRuQhkHD5S4?feature=shared)
* [Launch a webserver on EC2](https://www.youtube.com/watch?v=5wFUE61nBBE)
* [Launch a webserver on EC2 with a script](https://www.youtube.com/watch?v=Tv4Z_8Om7kw)

Goals: Create an EC2 instance, SSH into your instance, install a web server, and deploy a simple website.

Note: When creating your EC2 Instance, create a Security Group that allows SSH, HTTP, and HTTPS.

Here are the commands to run in your EC2 Instance.

```bash
sudo dnf update -y
sudo dnf install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

cd /var/www/html
sudo nano index.html # Add some HTML content here. (use ^O to save and ^X to exit)
```

Then you can navigate to your EC2 instance's public IP address in your browser to see the default Apache page. Note: Be sure you are using http:// and not https://. We have not set up SSL.

Challenge: Create an EC2 instance, clone one of your projects from GitHub, and deploy it using a web server, open the required PORT on the firewall. 

Here are some commands to get started...

```bash
sudo dnf update -y
sudo dnf install git -y
sudo dnf install -y nodejs
git clone https://github.com/your-username/your-project.git
cd your-project
npm install
nano .env # Add your environment variables here.
npm start 
```

Challenge: use the `pm2` command to run your server on startup.

Hint: use this prompt to ask an AI for help: "Instructions for cloning and setting up a node express server on an Amazon Linux 2023 AMI using dnf to install and ps2 to run it on startup in an EC2 instance."

```bash
sudo npm install -g pm2
pm2 start index.js
pm2 startup # Then run the generated command
pm2 save # Save the current process list to run on startup
```

Challenge: Use user data to install Apache and deploy a website on an EC2 instance.

```bash
#!/bin/bash
# User data script to install Apache and deploy a website on an EC2 instance.
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
sudo echo "<h1>Hello World, lets start this instance up!</h1>" > /var/www/html/index.html
```

Extension: After we have deployed a site on EC2, we can use Route 53 to point a domain to our EC2 instance. We can use a routing strategy to route traffic to different instances round-robin style.

## Day 4: IAM Roles and EC2 + S3

Goals: Create an IAM role that allows an EC2 instance to access an S3 bucket. SSH into the EC2 instance and use the CLI to interact with S3.

Challenge: Deploy a website on an EC2 instance that reads or writes to an S3 bucket. For example, you can use multer to save files to S3 from a form on your website.
