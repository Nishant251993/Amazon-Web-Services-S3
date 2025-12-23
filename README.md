<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Amazon S3 Integration – ASP.NET Core Web API</title>
    <style>
        body {
            font-family: Arial, Helvetica, sans-serif;
            line-height: 1.6;
        }
        h2, h3 {
            color: #232f3e;
        }
        pre {
            background: #f4f4f4;
            padding: 12px;
            overflow-x: auto;
        }
        ul {
            margin-left: 20px;
        }
    </style>
</head>
<body>

<h2>4. Register AWS Services in <code>Program.cs</code></h2>

<pre><code>
builder.Services.AddDefaultAWSOptions(
    builder.Configuration.GetAWSOptions());

builder.Services.AddAWSService&lt;IAmazonS3&gt;();
</code></pre>

<hr />

<h2>5. Configure AWS in <code>appsettings.json</code></h2>

<pre><code>
"AWS": {
  "Profile": "default",
  "Region": "ap-south-1"
}
</code></pre>

<ul>
    <li>Uses local AWS profile configured via AWS CLI</li>
    <li>Keeps AWS credentials secure</li>
</ul>

<hr />

<h2>6. Create Bucket Controller</h2>
<ul>
    <li>Create a new empty API controller</li>
    <li>Name it <strong>BucketsController</strong> (or any preferred name)</li>
</ul>

<hr />

<h2>7. Inject <code>IAmazonS3</code> via Constructor</h2>

<pre><code>
private readonly IAmazonS3 _s3Client;

public BucketsController(IAmazonS3 s3Client)
{
    _s3Client = s3Client;
}
</code></pre>

<hr />

<h2>8. Add POST API to Create S3 Bucket</h2>

<pre><code>
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
</code></pre>

<hr />

<h2>9. Create File Controller</h2>
<ul>
    <li>Create a new controller named <strong>FileController</strong></li>
    <li>Implement the following operations:</li>
    <ul>
        <li>Upload file to Amazon S3</li>
        <li>Download file from Amazon S3</li>
        <li>Delete file from Amazon S3 (optional)</li>
    </ul>
</ul>

<hr />

<h2>Features</h2>
<ul>
    <li>Amazon S3 bucket creation</li>
    <li>File upload, download, and delete operations</li>
    <li>Dependency Injection using <code>IAmazonS3</code></li>
    <li>Async and scalable ASP.NET Core Web API</li>
    <li>Configuration-based AWS setup</li>
</ul>

<hr />

<h2>Security Best Practices</h2>
<ul>
    <li>Do not hardcode AWS credentials</li>
    <li>Use IAM roles in production environments</li>
    <li>Grant least-privilege permissions to S3</li>
    <li>Enable server-side encryption if required</li>
</ul>

<hr />

<h2>Useful AWS Commands</h2>

<pre><code>
aws configure
aws s3 ls
</code></pre>

<hr />

<h2>Conclusion</h2>
<p>
This implementation provides a clean, scalable, and production-ready approach
to integrating <strong>Amazon S3</strong> with an <strong>ASP.NET Core Web API</strong>
using official AWS SDKs and best practices.
</p>

</body>
</html>
