# AWS S3 Multer integration

## Introduction

In this lesson, we will learn how to integrate AWS S3 with an Express application using Multer. We will create an API to upload files to an S3 bucket and retrieve them.

## Prerequisites

You should have an AWS account and your app with multer ready to go.

## Overview

1. **Set Up AWS S3 Bucket**:
   - Create an S3 bucket and configure it for public access.

2. **Install Required Packages**:

We will use Multer with AWS SDK for JavaScript (v3) to upload files to an S3 bucket. (v2 has been deprecated)

    - Install the following packages:

    ```bash
    npm install multer multer-s3
    npm @aws-sdk/client-s3
    ```

3. Create an AWS user with IAM permissions to access the S3 bucket.

  - Create a new user in the AWS Management Console.
  - Attach the `AmazonS3FullAccess` policy to the user.
    - **Note:** This is not recommended for production use. You should create a custom policy with the minimum required permissions.
  - Save the user's access key ID and secret access key.
  - Configure the S3 bucket to allow access from the user.

Here is an example policy to allow access to your user:

Replace `ACCOUNT_ID` with your AWS account ID and `USER_NAME` with your user's name. Replace the ARN of the bucket with the ARN of your bucket.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowIAMUserAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::ACCOUNT_ID:user/USER_NAME"
      },
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::coffee-images",
        "arn:aws:s3:::coffee-images/*"
      ]
    }
  ]
}

(Optional) Here is an example policy to attach to your user instead of S3FullAccess:


```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::coffee-images",
        "arn:aws:s3:::coffee-images/*"
      ]
    }
  ]
}
```

Save the user's access key ID and secret access key in your `.env` file.

Example `.env` file:

```env
AWS_REGION=us-east-2
AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY=YOUR_SECRET_ACCESS_KEY
AWS_S3_BUCKET=coffee-images
```

4. **Configure Multer to Upload Files to S3**:

Now all you need to do is configure Multer to upload files to your S3 bucket.

```js
// app.js
const multer = require('multer');
const multerS3 = require('multer-s3');
const { S3Client } = require('@aws-sdk/client-s3');
require('dotenv').config();

const s3 = new S3Client({
  region: process.env.AWS_REGION,
  credentials: {
    accessKeyId: process.env.AWS_ACCESS_KEY_ID,
    secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
  },
});

// Set storage engine
// Replace the diskStorage storage with s3 storage
// The rest of the code remains the same
const storage = multerS3({
  s3: s3,
  bucket: process.env.AWS_S3_BUCKET,
  key: function (req, file, cb) {
    cb(
      null,
      Date.now().toString() + '-' + file.fieldname + path.extname(file.originalname)
    );
  },
});

multer({
  storage: storage,
  ...
```

5. **Get the location of the uploaded file**:

After uploading the file, you can get the location of the file in the S3 bucket by accessing the `location` property of the file object.

You will have to change your POST and PUT routes to handle the file upload and update the image URL in the database.

```js
// Upload file to S3
router.post('/upload', upload, (req, res) => {
  // const imageUrl = req.file ? `/uploads/${req.file.filename}` : '';
  const imageUrl = req.file ? req.file.location : '';
});
```

6. **Test the File Upload API**:

You can now test the file upload API by sending a POST request with a file to the `/upload` endpoint.

## Summary

In this lesson, we learned how to integrate AWS S3 with an Express application using Multer. We created an API to upload files to an S3 bucket and retrieve them. This is a common use case for applications that require file storage and retrieval.
