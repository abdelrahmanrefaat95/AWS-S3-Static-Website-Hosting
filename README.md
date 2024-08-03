
# AWS - Static Website Hosting on AWS S3 using AWS CLI

This project provides a streamlined way to host your static websites (HTML, CSS, JavaScript, images, etc.) on Amazon S3, leveraging the power and cost-efficiency of AWS cloud infrastructure. By using the AWS Command Line Interface (CLI), you can automate the deployment process, making it easy to update your website with new content.

Who It's For:

Web Developers: Ideal for developers who want a simple and reliable way to deploy static websites or portfolios.
Bloggers: Great for personal blogs or small business websites that don't require dynamic content.
Anyone seeking cost-effective hosting: AWS S3's pricing model is very affordable for static website hosting.
Key Benefits:

Simple Setup: The AWS CLI simplifies the process of configuring your S3 bucket for website hosting.
Automated Deployment: Scripts can be used to automate the upload of your website files, saving you time.
Secure: AWS S3 provides robust security features to protect your website.
Scalable: AWS infrastructure scales effortlessly to handle traffic spikes.
Cost-Effective: S3's pay-as-you-go pricing is very budget-friendly for static websites.


## Project Overview: Deploying a Static Website with AWS S3

**Objective:**

The goal of this project was to leverage Amazon S3's static website hosting capabilities to deploy a website consisting of HTML, CSS, and JavaScript files. This approach offers a cost-effective, scalable, and secure solution for serving web content.

![Architecture Overview](https://photos.app.goo.gl/84YSik55Y8sU8vUo9)
## Documentation

[Hosting a static website using Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)


## Installation

Download and Install AWS CLI from official AWS website based on your preference of OS (Windows, Linux, MacOS)

[Install or update to the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
    

## Before you Start

1- Login into AWS Account and create IAM user with the required permissions to interact with AWS S3 service (Preferrably Admin User)

2- Create and Download AWS Access keys for that user to ineract with AWS using AWS CLI from your local machine.

3- Configure AWS Access keys using your preferred Terminal (PowerShell or Bash Shell) using The following Commands:

```
aws configure 

```
4- Follow the prompts on display and enter the following:

    AWS Access Key ID, AWS Secret Access Key, Default region name, Default output format

5- Download Static Website assests from any Source you          like, 
    I Used [html5up](https://html5up.net/)

[Static Website Download Link- ethereal](https://html5up.net/ethereal) 


## Deployment

Let's Start Doing the real work

**On AWS CLI**

1- Create S3 Bucket (Please be Aware of S3 Bucket name convention and make sure it's globally unique across all AWS Regions)
    
```
aws s3api create-bucket --bucket your-bucket-name --region your-bucket-region-code

```
2- Disable Block Public Access option on S3 Bucket you Created 
(This step is necessary for static website hosting)
```
aws s3api put-public-access-block --bucket your-bucket-name  --public-access-block-configuration BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false

```

3- Create The following Bucket Policy in JSON format to provide access to your website after being hosted and store it along with your website assests.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}

```

**This Bucket Policy Allows anyone to view your bucket objects using links which will be provided after hosting the website.**


4- Use The following Command to apply the bucket policy on your bucket.

```
aws s3api put-bucket-policy --bucket your-bucket-name --policy file://Path-to-policy-document

```

5- Enable Versioning on S3 Bucket:

```
aws s3api put-bucket-versioning --bucket your-bucket-name --versioning-configuration Status=Enabled

```
6- Enable Encrpytion on S3 Bucket by Creating encryption Policy document then apply it:

  **Encryption Policy document**

```
{
  "Rules": [
    {
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "AES256"
      }
    }
  ]
}

```
  **Apply The Policy**

```
aws s3api put-bucket-encryption --bucket your-bucket-name --server-side-encryption-configuration file://encryption-policy-location

```
5- Change Directory to your static website root folder, open Terminal in that location and upload your static website assests 

```
aws s3 sync Location-of-website-assets s3://s3-bucket-name/

```

6- Enable website hosting on S3 bucket by specifying the index page
and error page on S3 bucket using the following Commands:

```
aws s3 website s3://bucket-name --index-document [index-html-page] --error-document [error-html-page]

```

7- Verify your S3 Static website Hosting configuration

```
aws s3api get-bucket-website --bucket va-arefaat-vat6

```

8- Construct and access your website through that url:

```
http://your-bucket-name.s3-website-region-code.amazonaws.com/

```

9- Cleanup your resources:

Delete S3 Objects and its versions
```
aws s3 rm s3://your-bucket-name --recursive

```
Delete S3 Bucket itself

```
aws s3api delete-bucket --bucket your-bucket-name

```
## Usage and Examples

Link to my website Hosting on AWS S3 Bucket

[AWS-Static-website-hosting](http://va-arefaat-vat06.s3-website-us-east-1.amazonaws.com/)


## Thank You