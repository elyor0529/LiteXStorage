# LiteXStorage
> LiteXStorage is simple yet powerful and very high-performance storage mechanism and incorporating both synchronous and asynchronous usage with some advanced usages of cloud storage which can help us to handle storage more easier! 

Provide Storage service for ASP.NET Core (2.0 and later) applications.

Small library to abstract blob storage functionalities. Quick setup for any storage provider and very simple wrapper for the widely used storage providers. LiteX Storage uses the least common denominator of functionality between the supported providers to build a cross-cloud storage solution. Abstract interface to implement any kind of basic blob storage services. Having a default/generic implementation to wrap the Azure, Amazon AWS S3, Google Cloud, FileSystem (Local), Kvpbase storage. A cross-cloud storage abstraction. 

Very simple configuration in advanced ways. Purpose of this package is to bring a new level of ease to the developers who deal with different storage provider integration with their system and implements many advanced features. You can also write your own and extend it also extend existing providers. Easily migrate or switch between one to another provider with no code breaking changes.

It provides possibility to upload files, upload the content of a folder inclusively subfolders, enumerate buckets/container, enumerate the content of a specific 'subfolder', delete buckets/container and delete files from specific subfolders. 

LiteXStorage is an interface to unify the programming model for various storage providers. The Core library contains all base interfaces and tools. One should install at least one other LiteXStorage package to get storage handle implementation.


## Storage Providers :books:
- [Azure](docs/Azure.md)
- [AmazonS3](docs/Amazon.md)
- [Google Cloud](docs/Google.md)
- [FileSystem](docs/FileSystem.md)
- [Kvpbase](docs/Kvpbase.md) - deprecated


## Features :pager:
- Create container or bucket
- Get Shared Access Signature (SAS) URL
- Upload Blobb file
- Get Blob metadata
- Set Blob metadata
- Get all containers/buckets
- Delete all containers/buckets
- Delete directory/folder from container/bucket
- Get Container/Bucket size in bytes
- Get number to total items/files in Container/Bucket
- Async compatible
- Thread safe, concurrency ready
- Interface based API to support the test driven development and dependency injection
- Leverages a provider model on top of ILiteXBlobService under the hood and can be extended with your own implementation


## Basic Usage :page_facing_up:

### Step 1 : Install the package :package:

> Choose one kinds of sms provider type that you needs and install it via [Nuget](https://www.nuget.org/profiles/iamaashishpatel).
> To install LiteXSms, run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)

```Powershell
PM> Install-Package LiteX.Storage.Azure
PM> Install-Package LiteX.Storage.Amazon
PM> Install-Package LiteX.Storage.Google
PM> Install-Package LiteX.Storage.Local
PM> Install-Package LiteX.Storage.Kvpbase
```


### Step 2 : Configuration 🔨 
> Different types of storage provider have their own way to config.
> Here are samples that show you how to config.

##### 2.1 : AppSettings 
```js
{
  //LiteX Azure Storage settings
  "AzureBlobConfig": {
    "AzureBlobStorageConnectionString": "--- REPLACE WITH YOUR AZURE CONNECTION STRING ---",
    "AzureBlobStorageContainerName": "--- REPLACE WITH YOUR AZURE CONTAINER NAME ---",
    "AzureBlobStorageEndPoint": "--- REPLACE WITH YOUR AZURE END POINT ---",
    "EnableLogging": true
  },

  //LiteX Amazon Storage settings
  "AmazonBlobConfig": {
    "AmazonAwsAccessKeyId": "--- REPLACE WITH YOUR AMAZON ACCESS KEY ID ---",
    "AmazonAwsSecretAccessKey": "--- REPLACE WITH YOUR AMAZON SECRET ACCESS KEY ---",
    "AmazonRegion": "--- REPLACE WITH YOUR AMAZON REGION ---",
    "AmazonBucketName": "--- REPLACE WITH YOUR AZURE AMAZON BUCKET NAME ---",
    "EnableLogging": true
  },

  //LiteX Google Storage settings
  "GoogleCloudBlobConfig": {
    "GoogleProjectId": "--- REPLACE WITH YOUR GOOGLE PROJECT ID ---",
    "GoogleJsonAuthPath": "--- REPLACE WITH YOUR GOOGLE JSON AUTH PATH ---",
    "GoogleBucketName": "--- REPLACE WITH YOUR GOOGLE BUCKET NAME ---",
    "EnableLogging": true
  },

  //LiteX Kvpbase Storage settings
  "KvpbaseBlobConfig": {
    "KvpbaseApiKey": "--- REPLACE WITH YOUR KVPBASE API KEY ---",
    "KvpbaseContainer": "--- REPLACE WITH YOUR KVPBASE CONTAINER ---",
    "KvpbaseEndpoint": "--- REPLACE WITH YOUR KVPBASE END POINT ---",
    "KvpbaseUserGuid": "--- REPLACE WITH YOUR KVPBASE USERGUID ---",
    "EnableLogging": true
  },

  //LiteX Local File System Storage settings
  "FileSystemBlobConfig": {
    "Directory": "--- REPLACE WITH YOUR LOCAL FILE SYSTEM DIRECTORY ---",
    "EnableLogging": true
  }
}
```

##### 2.2 : Configure Startup Class
```cs
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        #region LiteX Storage (Azure)

        // 1. Use default configuration from appsettings.json's 'AzureBlobConfig'
        services.AddLiteXAzureBlobService();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXAzureBlobService(option =>
        {
            option.AzureBlobStorageConnectionString = "";
            option.AzureBlobStorageContainerName = "";
            option.AzureBlobStorageEndPoint = "";
            option.EnableLogging = true;
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var azureBlobConfig = new AzureBlobConfig()
        {
            AzureBlobStorageConnectionString = "",
            AzureBlobStorageContainerName = "",
            AzureBlobStorageEndPoint = "",
            EnableLogging = true
        };
        services.AddLiteXAzureBlobService(azureBlobConfig);

        #endregion

        #region LiteX Storage (Amazon)

        // 1. Use default configuration from appsettings.json's 'AmazonBlobConfig'
        services.AddLiteXAmazonBlobService();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXAmazonBlobService(option =>
        {
            option.AmazonAwsSecretAccessKey = "";
            option.AmazonAwsAccessKeyId = "";
            option.AmazonBucketName = "";
            option.AmazonRegion = "";
            option.EnableLogging = true;
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var amazonBlobConfig = new AmazonBlobConfig()
        {
            AmazonAwsSecretAccessKey = "",
            AmazonAwsAccessKeyId = "",
            AmazonBucketName = "",
            AmazonRegion = "",
            EnableLogging = true
        };
        services.AddLiteXAmazonBlobService(amazonBlobConfig);

        #endregion

        #region LiteX Storage (Google)

        // 1. Use default configuration from appsettings.json's 'GoogleCloudBlobConfig'
        services.AddLiteXGoogleCloudBlobService();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXGoogleCloudBlobService(option =>
        {
            option.GoogleProjectId = "";
            option.GoogleJsonAuthPath = "";
            option.GoogleBucketName = "";
            option.EnableLogging = true;
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var googleCloudBlobConfig = new GoogleCloudBlobConfig()
        {
            GoogleProjectId = "",
            GoogleJsonAuthPath = "",
            GoogleBucketName = "",
            EnableLogging = true
        };
        services.AddLiteXGoogleCloudBlobService(googleCloudBlobConfig);

        #endregion

        #region LiteX Storage (FileSystem-Local)

        // 1. Use default configuration from appsettings.json's 'FileSystemBlobConfig'
        services.AddLiteXFileSystemBlobService();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXFileSystemBlobService(option =>
        {
            option.Directory = "UploadFolder";
            option.EnableLogging = true;
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var fileSystemBlobConfig = new FileSystemBlobConfig()
        {
            Directory = "",
            EnableLogging = true
        };
        services.AddLiteXFileSystemBlobService(fileSystemBlobConfig);

        #endregion

        #region LiteX Storage (Kvpbase)

        // 1. Use default configuration from appsettings.json's 'KvpbaseBlobConfig'
        services.AddLiteXKvpbaseBlobService();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXKvpbaseBlobService(option =>
        {
            option.KvpbaseApiKey = "";
            option.KvpbaseEndpoint = "";
            option.KvpbaseContainer = "";
            option.KvpbaseUserGuid = "";
            option.EnableLogging = true;
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var kvpbaseBlobConfig = new KvpbaseBlobConfig()
        {
            KvpbaseApiKey = "",
            KvpbaseEndpoint = "",
            KvpbaseContainer = "",
            KvpbaseUserGuid = "",
            EnableLogging = true
        };
        services.AddLiteXKvpbaseBlobService(kvpbaseBlobConfig);

        #endregion


        // add logging (optional)
        services.AddLiteXLogging();
    }
}
```

### Step 3 : Use in Controller or Business layer :memo:

```cs
/// <summary>
/// Storage controller
/// </summary>
[Route("api/[controller]")]
public class StorageController : Controller
{
    #region Fields

    private readonly ILiteXBlobService _blobService;

    #endregion

    #region Ctor

    /// <summary>
    /// Ctor
    /// </summary>
    /// <param name="blobService"></param>
    public StorageController(ILiteXBlobService blobService)
    {
        _blobService = blobService;
    }

    #endregion

    #region Methods

    /// <summary>
    /// Get Storage Provider Type
    /// </summary>
    /// <returns></returns>
    [HttpGet]
    [Route("get-storage-provider-type")]
    public IActionResult GetStorageProviderType()
    {
        return Ok(_blobService.StorageProviderType.ToString());
    }


    /// <summary>
    /// Get blob list
    /// from default Container/Bucket
    /// </summary>
    /// <returns></returns>
    [HttpGet]
    [Route("get-blobs")]
    public async Task<IActionResult> GetBlobs()
    {
        List<BlobDescriptor> blobs = (await _blobService.GetBlobsAsync()).ToList();

        // sync
        //List<BlobDescriptor> blobs = _blobService.GetBlobs().ToList();

        return Ok(blobs);
    }

    /// <summary>
    /// Get blob list
    /// </summary>
    /// <returns></returns>
    [HttpGet]
    [Route("get-blobs/{containerOrBucketName}")]
    public async Task<IActionResult> GetBlobs(string containerOrBucketName)
    {
        List<BlobDescriptor> blobs = (await _blobService.GetBlobsAsync(containerOrBucketName)).ToList();

        // sync
        //List<BlobDescriptor> blobs = _blobService.GetBlobs(containerOrBucketName).ToList();

        return Ok(blobs);
    }


    /// <summary>
    /// Create/Replace blob file
    /// from default Container/Bucket
    /// </summary>
    /// <param name="file">Blob file</param>
    /// <param name="isPublic">Is Private or Public blob</param>
    /// <returns></returns>
    [HttpPost]
    [Route("upload-blob")]
    [AddSwaggerFileUploadButton]
    public async Task<IActionResult> UploadBlob(IFormFile file, bool isPublic = true)
    {
        string blobName = file.FileName;
        Stream stream = file.OpenReadStream();
        string contentType = file.ContentType;
        BlobProperties properties = new BlobProperties
        {
            ContentType = contentType,
            Security = isPublic ? BlobSecurity.Public : BlobSecurity.Private
        };

        var isUploaded = await _blobService.UploadBlobAsync(blobName, stream, properties);

        // sync
        //var isUploaded = _blobService.UploadBlob(blobName, stream, properties);

        return Ok(isUploaded);
    }

    /// <summary>
    /// Create/Replace blob file
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <param name="file">Blob file</param>
    /// <param name="isPublic">Is Private or Public blob</param>
    /// <returns></returns>
    [HttpPost]
    [Route("upload-blob/{containerOrBucketName}")]
    [AddSwaggerFileUploadButton]
    public async Task<IActionResult> UploadBlob(string containerOrBucketName, IFormFile file, bool isPublic = true)
    {
        string blobName = file.FileName;
        Stream stream = file.OpenReadStream();
        string contentType = file.ContentType;
        BlobProperties properties = new BlobProperties
        {
            ContentType = contentType,
            Security = isPublic ? BlobSecurity.Public : BlobSecurity.Private
        };

        var isUploaded = await _blobService.UploadBlobAsync(containerOrBucketName, blobName, stream, properties);

        // sync
        //var isUploaded = _blobService.UploadBlob(containerOrBucketName, blobName, stream, properties);

        return Ok(isUploaded);
    }


    /// <summary>
    /// Create/Replace blob file in directory/folder
    /// from default Container/Bucket
    /// </summary>
    /// <param name="directoryName">Name of directory/folder.</param>
    /// <param name="file">Blob file</param>
    /// <param name="isPublic">Is Private or Public blob</param>
    /// <returns></returns>
    [HttpPost]
    [Route("upload-blob-in-directory/{directoryName}")]
    [AddSwaggerFileUploadButton]
    public async Task<IActionResult> UploadBlobInDirectory(string directoryName, IFormFile file, bool isPublic = true)
    {
        string blobName = $"{directoryName}/{file.FileName}";
        Stream stream = file.OpenReadStream();
        string contentType = file.ContentType;
        BlobProperties properties = new BlobProperties
        {
            ContentType = contentType,
            Security = isPublic ? BlobSecurity.Public : BlobSecurity.Private
        };

        var isUploaded = await _blobService.UploadBlobAsync(blobName, stream, properties);

        // sync
        //var isUploaded = _blobService.UploadBlob(blobName, stream, properties);

        return Ok(isUploaded);
    }

    /// <summary>
    /// Create/Replace blob file in directory/folder
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <param name="directoryName">Name of directory/folder.</param>
    /// <param name="file">Blob file</param>
    /// <param name="isPublic">Is Private or Public blob</param>
    /// <returns></returns>
    [HttpPost]
    [Route("upload-blob-in-directory/{containerOrBucketName}/{directoryName}")]
    [AddSwaggerFileUploadButton]
    public async Task<IActionResult> UploadBlobInDirectory(string containerOrBucketName, string directoryName, IFormFile file, bool isPublic = true)
    {
        string blobName = $"{directoryName}/{file.FileName}";
        //string blobName = $"{directoryName}{Path.DirectorySeparatorChar}{file.FileName}";
        Stream stream = file.OpenReadStream();
        string contentType = file.ContentType;
        BlobProperties properties = new BlobProperties
        {
            ContentType = contentType,
            Security = isPublic ? BlobSecurity.Public : BlobSecurity.Private
        };

        var isUploaded = await _blobService.UploadBlobAsync(containerOrBucketName, blobName, stream, properties);

        // sync
        //var isUploaded = _blobService.UploadBlob(containerOrBucketName, blobName, stream, properties);

        return Ok(isUploaded);
    }


    /// <summary>
    /// Get blob file data (bytes or stream)
    /// from default Container/Bucket
    /// </summary>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("download-blob/{blobName}")]
    public async Task<IActionResult> DownloadBlob(string blobName)
    {
        // get blob
        Stream stream = await _blobService.GetBlobAsync(blobName);

        // sync
        //Stream stream = _blobService.GetBlob(blobName);

        var response = File(stream, "application/octet-stream", blobName); // FileStreamResult
        return response;
    }

    /// <summary>
    /// Get blob file data (bytes or stream)
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("download-blob/{containerOrBucketName}/{blobName}")]
    public async Task<IActionResult> DownloadBlob(string containerOrBucketName, string blobName)
    {
        // get blob
        Stream stream = await _blobService.GetBlobAsync(containerOrBucketName, blobName);

        // sync
        //Stream stream = _blobService.GetBlob(containerOrBucketName, blobName);

        var response = File(stream, "application/octet-stream", blobName); // FileStreamResult
        return response;
    }


    /// <summary>
    /// Get blob url
    /// from default Container/Bucket
    /// </summary>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("get-blob-url/{blobName}")]
    public async Task<IActionResult> GetBlobUrl(string blobName)
    {
        string blobUrl = await _blobService.GetBlobUrlAsync(blobName);

        // sync
        //string blobUrl = _blobService.GetBlobUrl(blobName);

        return Ok(blobUrl);
    }

    /// <summary>
    /// Get blob url
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("get-blob-url/{containerOrBucketName}/{blobName}")]
    public async Task<IActionResult> GetBlobUrl(string containerOrBucketName, string blobName)
    {
        string blobUrl = await _blobService.GetBlobUrlAsync(containerOrBucketName, blobName);

        // sync
        //string blobUrl = _blobService.GetBlobUrl(containerOrBucketName, blobName);

        return Ok(blobUrl);
    }


    /// <summary>
    /// Get blob SAS url
    /// from default Container/Bucket
    /// </summary>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("get-blob-sasurl/{blobName}")]
    public async Task<IActionResult> GetBlobSasUrl(string blobName)
    {
        string blobUrl = await _blobService.GetBlobSasUrlAsync(blobName, DateTimeOffset.UtcNow.AddHours(2), BlobUrlAccess.Read);

        // sync
        //string blobUrl = _blobService.GetBlobSasUrl(blobName, DateTimeOffset.UtcNow.AddHours(2), BlobUrlAccess.Read);

        return Ok(blobUrl);
    }

    /// <summary>
    /// Get blob SAS url
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("get-blob-sasurl/{containerOrBucketName}/{blobName}")]
    public async Task<IActionResult> GetBlobSasUrl(string containerOrBucketName, string blobName)
    {
        string blobUrl = await _blobService.GetBlobSasUrlAsync(containerOrBucketName, blobName, DateTimeOffset.UtcNow.AddHours(2), BlobUrlAccess.Read);

        // sync
        //string blobUrl = _blobService.GetBlobSasUrl(containerOrBucketName, blobName, DateTimeOffset.UtcNow.AddHours(2), BlobUrlAccess.Read);

        return Ok(blobUrl);
    }


    /// <summary>
    /// Delete blob file
    /// from default Container/Bucket
    /// </summary>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpDelete]
    [Route("delete-blob/{blobName}")]
    public async Task<IActionResult> DeleteBlobFile(string blobName)
    {
        var isDeleted = await _blobService.DeleteBlobAsync(blobName);

        // sync
        //var isDeleted = _blobService.DeleteBlob(blobName);

        return Ok(isDeleted);
    }

    /// <summary>
    /// Delete blob file
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpDelete]
    [Route("delete-blob/{containerOrBucketName}/{blobName}")]
    public async Task<IActionResult> DeleteBlobFile(string containerOrBucketName, string blobName)
    {
        var isDeleted = await _blobService.DeleteBlobAsync(containerOrBucketName, blobName);

        // sync
        //var isDeleted = _blobService.DeleteBlob(containerOrBucketName, blobName);

        return Ok(isDeleted);
    }


    /// <summary>
    /// Get blob metadata
    /// from default Container/Bucket
    /// </summary>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("get-blob-metadata/{blobName}")]
    public async Task<IActionResult> GetBlobMetadata(string blobName)
    {
        BlobDescriptor blobDescriptor = await _blobService.GetBlobDescriptorAsync(blobName);

        // sync
        //BlobDescriptor blobDescriptor = _blobService.GetBlobDescriptor(blobName);

        var metadata = blobDescriptor.Metadata;

        return Ok(blobDescriptor);
    }

    /// <summary>
    /// Get blob metadata
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("get-blob-metadata/{containerOrBucketName}/{blobName}")]
    public async Task<IActionResult> GetBlobMetadata(string containerOrBucketName, string blobName)
    {
        BlobDescriptor blobDescriptor = await _blobService.GetBlobDescriptorAsync(containerOrBucketName, blobName);

        // sync
        //BlobDescriptor blobDescriptor = _blobService.GetBlobDescriptor(containerOrBucketName, blobName);

        var metadata = blobDescriptor.Metadata;

        return Ok(blobDescriptor);
    }


    /// <summary>
    /// Set blob metadata
    /// from default Container/Bucket
    /// </summary>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpPut]
    [Route("set-blob-metadata/{blobName}")]
    public async Task<IActionResult> SetBlobMetadata(string blobName)
    {
        IDictionary<string, string> metadata = new Dictionary<string, string>();

        BlobProperties properties = new BlobProperties()
        {
            ContentType = "",
            Metadata = metadata,
            ContentDisposition = "",
            Security = BlobSecurity.Public
        };

        var isSet = await _blobService.SetBlobPropertiesAsync(blobName, properties);

        // sync
        //var isSet = _blobService.SetBlobProperties(blobName, properties);

        return Ok(isSet);
    }

    /// <summary>
    /// Set blob metadata
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <param name="blobName">Name of the Blob.</param>
    /// <returns></returns>
    [HttpPut]
    [Route("set-blob-metadata/{containerOrBucketName}/{blobName}")]
    public async Task<IActionResult> SetBlobMetadata(string containerOrBucketName, string blobName)
    {
        IDictionary<string, string> metadata = new Dictionary<string, string>();

        BlobProperties properties = new BlobProperties()
        {
            ContentType = "",
            Metadata = metadata,
            ContentDisposition = "",
            Security = BlobSecurity.Public
        };

        var isSet = await _blobService.SetBlobPropertiesAsync(containerOrBucketName, blobName, properties);

        // sync
        //var isSet = _blobService.SetBlobProperties(containerOrBucketName, blobName, properties);

        return Ok(isSet);
    }


    /// <summary>
    /// Delete directory/folder from default Container/Bucket
    /// </summary>
    /// <param name="directoryName">Name of directory/folder.</param>
    /// <returns></returns>
    [HttpDelete]
    [Route("delete-directory/{directoryName}")]
    public async Task<IActionResult> DeleteDirectory(string directoryName)
    {
        var isDeleted = await _blobService.DeleteDirectoryAsync(directoryName);

        // sync
        //var isDeleted = _blobService.DeleteDirectory(directoryName);

        return Ok(isDeleted);
    }

    /// <summary>
    /// Delete directory/folder from Container/Bucket
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <param name="directoryName">Name of directory/folder.</param>
    /// <returns></returns>
    [HttpDelete]
    [Route("delete-directory/{containerOrBucketName}/{directoryName}")]
    public async Task<IActionResult> DeleteDirectory(string containerOrBucketName, string directoryName)
    {
        var isDeleted = await _blobService.DeleteDirectoryAsync(containerOrBucketName, directoryName);

        // sync
        //var isDeleted = _blobService.DeleteDirectory(containerOrBucketName, directoryName);

        return Ok(isDeleted);
    }


    /// <summary>
    /// Get number to total items/files in Container/Bucket
    /// </summary>
    /// <returns></returns>
    [HttpGet]
    [Route("get-container-or-bucket-item-count")]
    public async Task<IActionResult> GetContainerOrBucketItemCount()
    {
        var itemsCount = await _blobService.GetContainerItemCountAsync();

        // sync
        //var itemsCount = _blobService.GetContainerItemCount();

        return Ok(itemsCount);
    }

    /// <summary>
    /// Get number to total items/files in Container/Bucket
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("get-container-or-bucket-item-count/{containerOrBucketName}")]
    public async Task<IActionResult> GetContainerOrBucketItemCount(string containerOrBucketName)
    {
        var itemsCount = await _blobService.GetContainerItemCountAsync(containerOrBucketName);

        // sync
        //var itemsCount = _blobService.GetContainerItemCount(containerOrBucketName);

        return Ok(itemsCount);
    }


    /// <summary>
    /// Get Container/Bucket size in bytes
    /// </summary>
    /// <returns></returns>
    [HttpGet]
    [Route("get-container-or-bucket-size")]
    public async Task<IActionResult> GetContainerOrBucketSize()
    {
        var size = await _blobService.GetContainerSizeAsync();

        // sync
        //var size = _blobService.GetContainerSize();

        return Ok(size);
    }

    /// <summary>
    /// Get Container/Bucket size in bytes
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("get-container-or-bucket-size/{containerOrBucketName}")]
    public async Task<IActionResult> GetContainerOrBucketSize(string containerOrBucketName)
    {
        var size = await _blobService.GetContainerSizeAsync(containerOrBucketName);

        // sync
        //var size = _blobService.GetContainerSize(containerOrBucketName);

        return Ok(size);
    }


    /// <summary>
    /// Create Container/Bucket
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <returns></returns>
    [HttpGet]
    [Route("create-container-or-bucket/{containerOrBucketName}")]
    public async Task<IActionResult> CreateContainerOrBucket(string containerOrBucketName)
    {
        var isCreated = await _blobService.CreateContainerAsync(containerOrBucketName);

        // sync
        //var isCreated = _blobService.CreateContainer(containerOrBucketName);

        return Ok(isCreated);
    }

    /// <summary>
    /// Delete Container/Bucket
    /// </summary>
    /// <param name="containerOrBucketName">Name of the Bucket/Container.</param>
    /// <returns></returns>
    [HttpDelete]
    [Route("delete-container-or-bucket/{containerOrBucketName}")]
    public async Task<IActionResult> DeleteContainerOrBucket(string containerOrBucketName)
    {
        var isDeleted = await _blobService.DeleteContainerAsync(containerOrBucketName);

        // sync
        //var isDeleted = _blobService.DeleteContainer(containerOrBucketName);

        return Ok(isDeleted);
    }



    /// <summary>
    /// Get all Containers/Buckets
    /// </summary>
    /// <returns></returns>
    [HttpGet]
    [Route("get-all-containers-or-buckets")]
    public async Task<IActionResult> GetAllContainersOrBuckets()
    {
        var containers = await _blobService.GetAllContainersAsync();

        // sync
        //var containers = _blobService.GetAllContainers();

        return Ok(containers);
    }

    /// <summary>
    /// Delete all Containers/Buckets
    /// </summary>
    /// <returns></returns>
    [HttpDelete]
    [Route("delete-all-containers-or-buckets")]
    public async Task<IActionResult> DeleteAllContainersOrBuckets()
    {
        var isDeleted = await _blobService.DeleteAllContainersAsync();

        // sync
        //var isDeleted = _blobService.DeleteAllContainers();

        return Ok(isDeleted);
    }

    #endregion
}
```


## Todo List :clipboard:

#### Storage Providers

- [x] Azure
- [x] AmazonS3
- [x] Google Cloud
- [x] File System
- [x] Kvpbase

#### Basic Storage API

- [x] Get Blob SAS



#### Coming soon
- Obsolete sync methods
- Multiple provider support (using provider factory)
- Get Container/Bucket size in bytes
- Get number of total items/files in Container/Bucket


---



## Support :telephone:
> Reach out to me at one of the following places!

- Email :envelope: at <a href="mailto:toaashishpatel@gmail.com" target="_blank">`toaashishpatel@gmail.com`</a>
- NuGet :package: at <a href="https://www.nuget.org/profiles/iamaashishpatel" target="_blank">`@iamaashishpatel`</a>



## Authors :boy:

* **Ashish Patel** - [A-Patel](https://github.com/a-patel)


##### Connect with me

| Linkedin | GitHub | Facebook | Twitter | Instagram | Tumblr | Website |
|----------|----------|----------|----------|----------|----------|----------|
| [![linkedin](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-linkedin.svg)](https://www.linkedin.com/in/iamaashishpatel) | [![github](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-github.svg)](https://github.com/a-patel) | [![facebook](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-facebook.svg)](https://www.facebook.com/aashish.mrcool) | [![twitter](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-twitter.svg)](https://twitter.com/aashish_mrcool) | [![instagram](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-instagram.svg)](https://www.instagram.com/iamaashishpatel/) | [![tumblr](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-tumblr.svg)](https://iamaashishpatel.tumblr.com/) | [![website](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-blogger.svg)](http://aashishpatel.co.nf/) |
| | | | | | |



## Donations :dollar:

[![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/iamaashishpatel) 

[![Patreon](https://c5.patreon.com/external/logo/become_a_patron_button.png)](https://www.patreon.com/iamaashishpatel)



## License :lock:

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

