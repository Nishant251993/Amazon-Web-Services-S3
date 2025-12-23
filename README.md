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

### 4. Register AWS Services

Open the **Program.cs** file of your ASP.NET Core Web API project.

Register AWS services after creating the application builder and before building the application.

```csharp
builder.Services.AddDefaultAWSOptions(
    builder.Configuration.GetAWSOptions());

builder.Services.AddAWSService<IAmazonS3>();

