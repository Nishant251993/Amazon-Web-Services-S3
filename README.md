🚀 Amazon S3 Integration with ASP.NET Core Web API

This project demonstrates how to integrate Amazon S3 with an ASP.NET Core Web API to perform common bucket and file operations such as creating buckets and uploading files using AWS SDK for .NET.

📌 Prerequisites

.NET 6 / 7 / 8 SDK

AWS Account

AWS IAM user with S3 permissions

AWS credentials configured locally (aws configure)

🛠️ Step-by-Step Implementation
✅ 1. Create ASP.NET Core Web API Project

Create a new ASP.NET Core Web API project using Visual Studio or CLI.

✅ 2. Install Required NuGet Package

Install AWS Core setup package:

NuGet Package Manager

Amazon.Extensions.NETCore.Setup

✅ 3. Install AWS S3 SDK

Run the following command in Package Manager Console:

Install-Package AWSSDK.S3

✅ 4. Configure AWS Services in Program.cs
builder.Services.AddDefaultAWSOptions(
    builder.Configuration.GetAWSOptions());

builder.Services.AddAWSService<IAmazonS3>();


This enables AWS dependency injection and S3 service registration.

✅ 5. Add AWS Configuration in appsettings.json
"AWS": {
  "Profile": "default",
  "Region": "ap-south-1"
}


🔹 Ensure the AWS profile exists on your machine
🔹 Example: aws configure

✅ 6. Create Bucket Controller

Create a new empty API controller named BucketsController
(You may choose any name).

✅ 7. Inject IAmazonS3 via Constructor
private readonly IAmazonS3 _s3Client;

public BucketsController(IAmazonS3 s3Client)
{
    _s3Client = s3Client;
}

✅ 8. Add POST Method to Create S3 Bucket
[HttpPost]
public async Task<IActionResult> CreateBucketAsync(string bucketName)
{
    var bucketExists = await Amazon.S3.Util.AmazonS3Util
        .DoesS3BucketExistV2Async(_s3Client, bucketName);

    if (bucketExists)
    {
        return BadRequest($"Bucket {bucketName} already exists.");
    }

    await _s3Client.PutBucketAsync(bucketName);

    return Created("bucket", $"Bucket {bucketName} created successfully.");
}

✅ 9. Create File Controller (Upload / Download Operations)

Create a new controller named FileController to handle:

File upload to S3

File download from S3

File deletion (optional)

Example operations you can implement:

UploadFileAsync

DownloadFileAsync

DeleteFileAsync

📂 Features Implemented

✔ AWS S3 Bucket Creation
✔ Dependency Injection using IAmazonS3
✔ Clean configuration using appsettings.json
✔ Async & scalable API design

🔐 Security Best Practices

Do NOT hardcode AWS credentials

Use IAM roles in production

Restrict S3 permissions (least privilege)

Enable server-side encryption if required

📘 Useful AWS Commands
aws configure
aws s3 ls

📌 Conclusion

This setup provides a clean, scalable, and production-ready approach to integrating Amazon S3 with ASP.NET Core Web API using official AWS SDKs.

⭐ If you find this helpful, feel free to star the repository and contribute!
