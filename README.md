# ☁️ Amazon S3 Integration – ASP.NET Core Web API

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" width="120" />
</p>

This project demonstrates how to integrate **Amazon S3** with an **ASP.NET Core Web API** using the official **AWS SDK for .NET**.

---

## 🚀 Steps

---

### 1. Create ASP.NET Core Web API Project
- Create a new **ASP.NET Core Web API** project using Visual Studio or CLI.

---

### 2. Install AWS Core Package
- Open **NuGet Package Manager**
- Install the following package:
  - `Amazon.Extensions.NETCore.Setup`

---

### 3. Install AWS S3 SDK
Run the following command in **Package Manager Console**:

```powershell
Install-Package AWSSDK.S3

4. Register AWS Services in Program.cs
builder.Services.AddDefaultAWSOptions(
    builder.Configuration.GetAWSOptions());

builder.Services.AddAWSService<IAmazonS3>();

5. Configure AWS in appsettings.json
"AWS": {
  "Profile": "default",
  "Region": "ap-south-1"
}


Uses local AWS profile

Keeps credentials secure

6. Create Bucket Controller

Create a new empty API controller

Name it BucketsController (or any name)

7. Inject IAmazonS3 via Constructor
private readonly IAmazonS3 _s3Client;

public BucketsController(IAmazonS3 s3Client)
{
    _s3Client = s3Client;
}

8. Add POST API to Create S3 Bucket
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

9. Create File Controller

Create a controller named FileController

Implement the following operations:

Upload file to Amazon S3

Download file from Amazon S3

Delete file from Amazon S3 (optional)

📂 Features

Amazon S3 bucket creation

File upload, download, and delete operations

Dependency Injection using IAmazonS3

Async and scalable ASP.NET Core Web API

Configuration-based AWS setup

🔐 Security Best Practices

Do not hardcode AWS credentials

Use IAM roles in production

Apply least-privilege permissions

Enable server-side encryption if required

🧰 Useful AWS Commands
aws configure
aws s3 ls

📘 Conclusion

This project provides a clean, scalable, and production-ready approach to integrating Amazon S3 with ASP.NET Core Web API using official AWS SDKs and best practices.
