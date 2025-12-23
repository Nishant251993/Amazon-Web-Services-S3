<p align="center" style="font-size:14px; font-weight:bold;">
  ☁️ #Amazon S3 Integration – ASP.NET Core Web API
</p>

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

- Install the following package:
Install-Package AWSSDK.S3

---

### 4. Register AWS Services

Open the **Program.cs** file of your ASP.NET Core Web API project.

Register AWS services after creating the application builder and before building the application.


builder.Services.AddDefaultAWSOptions(
    builder.Configuration.GetAWSOptions());

builder.Services.AddAWSService<IAmazonS3>();

---

### 5. Configure AWS Settings

Open the **appsettings.json** file.

Add AWS configuration such as profile name and region.
This configuration tells the application which AWS account and region to use.

"AWS": {
  "Profile": "default",
  "Region": "ap-south-1"
}

---

### 6. Create Bucket Controller

Inside the **Controllers** folder, create a new empty API controller.

Name the controller **BucketsController**.
This controller will be responsible for bucket-related operations.

---

### 7. Inject Amazon S3 Client

Open the **BucketsController** file.

Inject the Amazon S3 client using constructor injection.
This allows the controller to communicate with Amazon S3 services.
private readonly IAmazonS3 _s3Client;
public BucketsController(IAmazonS3 s3Client)
{
    _s3Client = s3Client;
}

---

### 8. Add API to Create S3 Bucket

Inside the **BucketsController**, add an API method to create an Amazon S3 bucket.

The API should first check whether the bucket already exists.
If the bucket does not exist, it should create a new bucket.

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

---

### 9. Create File Controller

Inside the **Controllers** folder, create another controller named **FileController**.

This controller will handle file-related operations such as uploading and downloading files.

---

### 10. Implement File Operations

Use the **FileController** to implement the following features:
Upload files to Amazon S3  
Download files from Amazon S3  
Delete files from Amazon S3 (optional)

---

### 11. Follow Security Best Practices

Do not hardcode AWS credentials in the application.
Use IAM roles or AWS profiles wherever possible.
Grant only minimum required permissions to Amazon S3 resources.

---

### 12. Test the Integration

Run the application and test the APIs using Swagger or Postman.

Verify that buckets are created successfully and files are uploaded to Amazon S3.

---

### Conclusion

By following these steps, Amazon S3 can be successfully integrated with an ASP.NET Core Web API.
This setup is scalable, secure, and suitable for production use.

