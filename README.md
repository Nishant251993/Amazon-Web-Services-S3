# ☁️ Amazon S3 Integration – ASP.NET Core Web API

## 🚀 Steps

### 1. Create ASP.NET Core Web API Project
- Create a new ASP.NET Core Web API project.

### 2. Install AWS Core Package
- Install `Amazon.Extensions.NETCore.Setup` from NuGet.

### 3. Install AWS S3 SDK
```powershell
Install-Package AWSSDK.S3

<h2>4. Register AWS Services in <code>Program.cs</code></h2>

<pre>
builder.Services.AddDefaultAWSOptions(
    builder.Configuration.GetAWSOptions());

builder.Services.AddAWSService&lt;IAmazonS3&gt;();
</pre>

<hr/>

<h2>5. Configure AWS in <code>appsettings.json</code></h2>

<pre>
"AWS": {
  "Profile": "default",
  "Region": "ap-south-1"
}
</pre>

<ul>
  <li>Uses local AWS profile configured via AWS CLI</li>
  <li>Keeps AWS credentials secure</li>
</ul>

<hr/>

<h2>6. Create Bucket Controller</h2>

<ul>
  <li>Create a new empty API controller</li>
  <li>Name it <strong>BucketsController</strong> (or any name)</li>
</ul>

<hr/>

<h2>7. Inject <code>IAmazonS3</code> via Constructor</h2>

<pre>
private readonly IAmazonS3 _s3Client;

public BucketsController(IAmazonS3 s3Client)
{
    _s3Client = s3Client;
}
</pre>

<hr/>

<h2>8. Add POST API to Create S3 Bucket</h2>

<pre>
[HttpPost]
public async Task&lt;IActionResult&gt; CreateBucketAsync(string bucketName)
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
</pre>

<hr/>

<h2>9. Create File Controller</h2>

<ul>
  <li>Create a new controller named <strong>FileController</strong></li>
  <li>Implement below operations:</li>
  <ul>
    <li>Upload file to Amazon S3</li>
    <li>Download file from Amazon S3</li>
    <li>Delete file from Amazon S3 (optional)</li>
  </ul>
</ul>

<hr/>

<h2>Features</h2>

<ul>
  <li>Amazon S3 bucket creation</li>
  <li>File upload, download, and delete operations</li>
  <li>Dependency Injection using <code>IAmazonS3</code></li>
  <li>Async and scalable ASP.NET Core Web API</li>
  <li>Configuration-based AWS setup</li>
</ul>

<hr/>

<h2>Security Best Practices</h2>

<ul>
  <li>Do not hardcode AWS credentials</li>
  <li>Use IAM roles in production</li>
  <li>Apply least-privilege permissions for S3</li>
  <li>Enable server-side encryption if required</li>
</ul>

<hr/>

<h2>Useful AWS Commands</h2>

<pre>
aws configure
aws s3 ls
</pre>

<hr/>

<h2>Conclusion</h2>

<p>
This implementation provides a clean, scalable, and production-ready approach
to integrating <strong>Amazon S3</strong> with an <strong>ASP.NET Core Web API</strong>
using official AWS SDKs and best practices.
</p>

