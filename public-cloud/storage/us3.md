---
layout: default
title: US3
parent: Storage
grand_parent: Public Cloud
permalink: /public-cloud/storage/us3/
nav_order: 2
---
# Object storage (US3)
## Product overview
Object storage (US3) is a service that provides unstructured data storage for Internet applications. Compared with traditional hard disk storage, object storage has the advantages of unlimited storage, high concurrent access, and lower cost. Its data durability is at least `99.9999999999%` and the availability of standard storage services is at least `99.95%`.

You can easily move massive amounts of data into or out of US3 using the APIs, SDK interfaces, or US3 migration tools. After the data is stored in US3, you can select the standard storage type of US3 service as the main storage method for mobile applications, large websites, image sharing, or audio and video. You can also choose the US3 service of the INAC storage type and archive storage type with lower cost and longer storage period as backup and archive for infrequently accessed data.

## Key concepts
### Object storage (bucket)

Object storage space (referred to as storage space) is the organization and management unit of files, and a file must belong to a certain space. Space names are globally unique and cannot be modified. Each account can create up to 20 buckets, and the number of files in the storage space is unlimited. Users can set a storage space to public or private to control access to files within the storage space.

### Private space

All files and all operations must be authorized by the owner's API key to be accessed.

### Public space

All file downloads are accessible directly via URL. Uploading, deleting, listing, or API key authorization is required for access.

### Object/File

A file is a logical unit of storage for a storage space. For each account, each file stored in the account is identified by a unique pair of buckets and keys.

### File name (Key)
The file name is the name of the corresponding file, which is globally unique in the storage space, and each file name identifies a file in the storage space, and the user can customize the file name when writing the file. 

Uploading a file with the same file name will cause the original file name to be overwritten. When downloading a file, users only need to know the domain name of the download export, without knowing which device in which computer room the file will be stored in, nor do they need to know the specific storage form. Just enter the corresponding URL in the browser to access.
File name naming convention
- Use UTF-8 encoding
- The length must be between 1-1023 bytes
- You can start with the `"/"` character, but `"{}^[]<>#~%"` is not allowed.

### Access domain name (Endpoint)
Endpoint represents the domain name of US3 access to external services. US3 provides services in the form of HTTP RESTful APIs, which require different domain names when accessing different regions. The domain names required to access the same region through the intranet and the Internet are also different. For more information, see Region and domain name.
### Token Key (API Access)
Token key is a pair of public and private keys; Users can create token keys, grant different token permissions to buckets, and distribute different tokens to different users to achieve fine-grained permission management on buckets. In addition, the token key can be set to a validity period or deleted at any time to ensure the security of accessing the bucket.
### API Key (API Access)
After a user registers a UCloud account, the system generates an API key for the user to identify the user. The API key is used to authenticate when calling the API to prevent others from maliciously tampering with your request data, such as the key leakage, please reset it immediately, and you need to log out of the website and log in again after the reset is successful. An API key consists of two parts: a public key and a private key. An API request can be made using the public and private keys to generate a signature. To ensure the security of your account, please keep your private key properly and avoid external transmission. Try not to directly use API keys to access US3, which is more risky after leakage, and we recommend that you use token keys.
### Region
The region represents the physical location of the US3 data center. You can select the region where the data is stored based on the cost and the source of the request. For more information, see Region activated for US3.
### Single-region space management
Single-region object storage service can solve the file storage problem of business architecture, create multiple copies of data uploaded by users, and realize cross-data center storage.
### Multi-region cross-region replication
You can configure cross-region replication for two or more buckets specified to implement data upload and synchronization in multiple regions and achieve multi-region data backup and disaster recovery. By using custom domain names and `cnames` to rotate cross-region replication to multiple buckets, the function of nearby upload can also be realized.
### Related Services
Once you have stored your data in US3, you can use other products and services provided by UCloud to manipulate it. Here are the UCloud products and services you'll use regularly:

| Products | Service |
| --- | --- |
| Cloud host (UHost). | Provide simple, efficient, flexible and scalable cloud computing services. |
| Content Distribution Network (UCDN). | Slowly store origin server resources to edge nodes in each region so that you can quickly get content nearby. |
| Data Lake Analytics (USQL). | Build serverless-based big data analytics solutions that make it easy for you to analyze and manipulate your data. |
| Media Processing (UMedia). | Transcode UFile-stored audio and video into formats suitable for playback on PC, TV, and mobile terminals. And based on the in-depth learning of massive data, multi-modal analysis of audio and video content, text, voice and scene. Realize intelligent review, content understanding, and intelligent editing. |

### Use US3
US3 provides Web services pages for you to manage US3. You can log in to the US3 Management Console to manipulate storage spaces and objects. For more information about the management console, see Console User Guide.
US3 also provides rich API interfaces and SDK packages in various languages to facilitate you to manage US3 flexibly. See API List and SDK List.
### US3 pricing
Traditional storage service providers require you to purchase a predetermined amount of storage and network transmission capacity, and if this capacity is exceeded, the corresponding service will be shut down or charged a high overcapacity fee. If this capacity is not exceeded, you will be charged for the full capacity. US3 only charges for what you use, you don't need to buy storage and data capacity upfront, and you will enjoy more infrastructure cost advantages as your business grows.

## Product advantages
### Stable and reliable storage capabilities
Based on the high-availability architecture design, multiple copies of user-stored files are distributed in different storage clusters, and even if a cluster-scale failure occurs, the availability of stored files will not be affected, providing 11 `9s` of data durability. The storage service supports high concurrent access, breaks through the limitations of traditional disk I/O, and meets the online storage requirements of users with high access and high download volume.
### Low-cost elastic storage
There is no upper limit on cloud storage space, no need to consider storage space expansion, and support single files of less than or equal to `5TB`. It is suitable for mass file storage in UGC applications such as audio and video, and image sharing. Billing is based on actual usage, with no minimum fees, no idle waste of storage and bandwidth resources.
### Flexible and convenient service access
Provides multiple access methods such as APIs/SDKs, command-line tools, and consoles, and is compatible with AWS S3 APIs for multiple languages. It helps users seamlessly access their original services, greatly shorten the development cycle, and help them quickly go online. In close collaboration with UCloud's various products, UHost users can access the intranet through the cloud host, providing stable and high-speed intranet upload and download speed.
### Easy-to-use management tools
Provides a variety of management tools such as the visual page US3Browser and the command line US3CLI to manage storage space and files, making the operation simple and easy to use. The US3FS tool is available to mount storage spaces locally and manipulate object storage as if it were a local file system. Provide big data adaptation tools to solve the problem of Hadoop accessing object storage, and support big data computing frameworks such as `Hive`, `Spark`, and `Flink` to read and write data on object storage just like accessing the `HDFS` file system. US3SYNC is provided as a migration tool to easily migrate data from on-premises or other cloud environments to US3 storage.
### Secure and reliable distributed system
Provide functions such as anti-hotlink, client-side encryption, server-side encryption, and token whitelisting; Support SSL encrypted transmission; The UCloud attack defense system is supported to defend against DDos attacks.
### Diverse cloud application scenarios
Adapts to various UCloud public cloud services to provide stable and reliable back-end storage support for various services. Combined with CDN, it solves the storage and distribution of massive data, effectively reduces access latency, and improves the access experience of end users across the network. It supports docking image processing and video stream processing services, and provides users with various multimedia online processing capabilities. It can also be used as a data source to provide massive dataset support for scenarios such as big data analysis and AI training inference.
## Usage Restrictions
### Supported regions

| Region | Single-region space management | Cross-region replication |
| --- | --- | --- |
| North China One |  Supported |  Supported |
| North China II |  Supported | Not supported |
| Shanghai II |  Supported |  Supported |
| Guangzhou |  Supported |  Supported |
| Hong Kong |  Supported | Not supported |
| Los Angeles |  Supported | Not supported |
| Singapore |  Supported | Not supported |
| Jakarta |  Supported | Not supported |
| Taipei |  Supported | Not supported |
| Lagos |  Supported | Not supported |
| St. Paulo |  Supported | Not supported |
| Dubai |  Supported | Not supported |
| Frankfurt |  Supported | Not supported |
| Ho Chi Minh City |  Supported | Not supported |
| Washington |  Supported | Not supported |
| Mumbai |  Supported | Not supported |
| Seoul |  Supported | Not supported |
| Tokyo |  Supported | Not supported |
| Bangkok |  Supported | Not supported |

### HTTPS access restrictions

Supports HTTPS access to ensure transmission security.

Note: Currently, HTTPS is not supported for mirror back-to-origin and custom domain names.

### Upload file size limit

Upload through the console, upload file PutFile, upload table upload PostFile, and second upload. The file size of the file UploadHit cannot exceed `512MB`, and the file size of the file exceeding `512MB` must be uploaded The shard upload method must be used.

### Image handling restrictions

| Region | Size limit | Concurrency limit |
| --- | --- | --- |
| Beijing | 20M | 50 |
| Shanghai II | 20M | 10 |
| Guangzhou | 20M | 10 |
| Hong Kong | 20M | 10 |
| Taipei | 20M | 10 |
| Other regions | 20M | 5 |

*Remarks: If you have greater needs,please contact the customer manager or technical support.*

### Other restrictions

| Restrictions | Illustrate |
| --- | --- |
| Archived storage type | It needs to be thawed before access, and the stored data is restored from the frozen state to the readable state Depending on the file size and type of thawing, the waiting time is different; <br/> At present, the Hong Kong, Taipei and overseas availability zones of US3 storage and storage types include: Hong Kong, Los Angeles, Singapore, Taipei, Ho Chi Minh City, Washington, Tokyo, and Seoul. |
| Bucket | The total number of storage spaces created in the same region with the same account number cannot exceed 20. |
| ::: | Once the storage space is successfully created, its name, region, and storage type cannot be modified. |
| ::: | The capacity of a single storage room is unlimited. |
| Upload/download files | The default threshold for the bandwidth of the same account number in the same region is as follows: <br/>Upload and download on the intranet speed: `10Gbit/s` in mainland China, `1Gbit/s` in other regions; <br/> Upload speed on the Internet: `1Gbit/s`. <br/>If your business (such as offline data processing) requires greater bandwidth, contact technical support. |
| QPS (write). | Ordinary users `1000` requests per second (not the upper limit), overseas `100`, if your business has greater needs, please contact the technology  Supported. |
| QPS (read). | `2000` requests per second for ordinary users (not the upper limit), `100` overseas, if your business has greater needs, please contact the technology  Supported. |
| Delete files | Files cannot be recovered after deletion. The maximum number of files deleted in batches in the console is `1000` and `100` overseas  Supported. |
| Domain name binding | Domain names tied to the mainland region of China must be filed with the Ministry of Industry and Information Technology, and domain names tied in other regions do not need to be filed with the Ministry of Industry and Information Technology. Up to `100` domains can be tied to each storage room. |

## Storage type
US3 provides three storage types: standard, in-frequency, and archive, which are used for frequently accessed hot data, low-frequency access backup data, and archived data suitable for long-term storage, covering various data storage scenarios from hot to cold.

### Standard storage
Standard Storage provides highly reliable, highly available, and high-performance object storage services with high throughput and low latency service response performance, and can support frequent hotspot data access.

Key features:
- Data durability: `99.9999999999%` 
- Availability: `99.95%`
- Access: Real-time access
- Applicable scenarios: various social and sharing pictures, audio and video applications, large websites, big data analysis, mobile applications, and game programs.

### IA storage
IA storage provides object storage services with high reliability, low storage cost, and low access latency, which is suitable for long-term storage of infrequently accessed data. The unit price of storage is lower than that of the standard type, IA storage has a minimum storage time and minimum object size, and a storage time of less than 30 days will incur certain fees for early deletion. If the individual file size is less than 64KB, the storage space is calculated at 64KB, and data ingestion incurs charges.

Key features:
- Data durability: `99.9999999999%`
- Availability: `99.9%`
- Access: Real-time access
- Applicable scenarios: Long-term backup of various mobile applications, smart devices, government and enterprise affairs data, and enterprise data, and real-time data access.

### Archive storage
The archive storage type provides offline cold data storage with high reliability, extremely low storage cost and long-term storage, suitable for archived data that needs long-term storage (recommended for more than half a year), and is rarely accessed during the storage cycle. 

The unit price is the lowest among the three storage types, archive storage has the shortest storage time and minimum object size, and early deletion of files with a storage time of less than 60 days will incur a certain fee. 

If the file size is less than 64KB, the storage space will be calculated as 64KB, and data acquisition will incur charges. Currently, the archive type is limited to Chinese mainland zones (China North 1, China North 2, Shanghai, and Guangzhou).

Key features:
- Data durability: 99.9999999999%
- Availability: 99.9%
- Access: Thawing is required before access, and the waiting time required to return from frozen to readable state varies depending on the file size and type of thawing, please refer to the product price supplement
- Applicable scenarios: long-term storage of archival data and other compliance documents, medical images, scientific materials, film and television materials
### Storage type conversion
Automatic conversion of the following storage types is supported:

- Standard storage is converted to IA storage
- Standard storage is converted to archive storage
- IA storage is converted to archival storage

Remark:

- For files that need to be converted to standard or IA files, and IA files are converted to standard files, you can convert the storage type to file by uploading the file again and specifying the corresponding storage type. <br/>
For example, if you need to convert a file that has been stored as an IA type back to a standard type, you can upload the file again, specify the standard storage type when uploading, and the newly written file is the standard storage type.

- For files that have been dumped into an archive type, you need to perform a RESTORE operation and thaw them to a readable state before they can be read.


