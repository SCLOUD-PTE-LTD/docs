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


