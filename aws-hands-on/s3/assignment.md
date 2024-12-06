**Assignment Title: Create and Configure an S3 Bucket for Public Image Hosting**

**Summary:**\
In this 1-hour assignment, you will learn how to create an Amazon S3 bucket, upload an image to it, and make it publicly accessible via HTTPS. You will use this bucket as a storage solution for hosting an image that can be used on a website.

**Learning Objectives:**

- Create an S3 bucket on AWS.
- Configure permissions to make an S3 bucket public.
- Upload an image to the bucket and make it accessible via HTTPS.

**Instructions:**

### Step 1: Set Up an S3 Bucket

1. **Sign in to AWS Console:** Go to [AWS Management Console](https://aws.amazon.com/console/).
2. **Navigate to S3 Service:** In the search bar, type **S3** and select **Amazon S3** from the list of services.
3. **Create a New Bucket:** Click on **Create bucket**.
   - **Bucket Name:** Enter a unique name for your bucket (e.g., `my-website-images-123`).
   - **Region:** Choose a region that is closest to your users for better performance.
   - Leave the other settings as default for now.
   - Click **Create bucket**.

### Step 2: Configure Bucket Permissions

1. **Enable Public Access:**

   - In the **S3 Dashboard**, click on your newly created bucket.
   - Go to the **Permissions** tab.
   - Scroll down to **Block Public Access** settings and click **Edit**.
   - Uncheck **"Block all public access"** and confirm that you want to allow public access.
   - Click **Save**.

2. **Add a Bucket Policy Using the Console:**

   - Scroll further down to the **Bucket policy** section and click **Edit**.
   - Click on **Policy Generator** to create the policy.
   - **Configure the Policy Generator:**
     - **Select Policy Type:** Choose **S3 Bucket Policy**.
     - **Principal:** Enter `*` to allow access from any user.
     - **Actions:** Select **GetObject** from the dropdown list.
     - **ARN:** Enter the bucket ARN in the format: `arn:aws:s3:::my-website-images-123/*` (replace `my-website-images-123` with your bucket name).
       - Note: The ARN is the Amazon Resource Number of your S3 bucket.&#x20;
       - You can copy and paste the ARN from the Properities tab of the S3 bucket. Be sure to add a /\* to the end to indicate a wildcard.
     - Click **Add Statement**, then **Generate Policy**.
   - Copy the generated policy into the **Bucket policy** editor.
   - Click **Save changes**.

### Step 3: Upload an Image

1. **Upload an Image File:**
   - Go to the **Objects** tab and click **Upload**.
   - Click **Add files** and select an image file from your computer.
   - Click **Upload**.

### Step 4: Make the Image Available via HTTPS

1. **Get the Image URL:**

   - Once the image is uploaded, click on the file name to open its details.
   - You will see the **Object URL**, which looks something like: `https://my-website-images-123.s3.amazonaws.com/your-image.jpg`.
   - Copy this URL.

2. **Test the Image URL:**

   - Open a new browser tab and paste the URL.
   - You should see the image load. This means it is publicly accessible and available via HTTPS.

**Deliverable:**\
Submit the public URL of your image along with a screenshot of your S3 bucket's permissions page to show that public access is enabled.

**Tips:**

- Make sure you use a unique bucket name, as S3 bucket names must be globally unique.
- Double-check your permissions if the image does not load publicly; ensuring the bucket policy is correct is crucial.

