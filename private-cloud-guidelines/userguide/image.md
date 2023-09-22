---
layout: default
title: Image Management
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/image-management/
nav_order: 4
---
# 4. Image Management

Self-made images and ISO images belong to the cloud platform tenant. Images exported from virtual machines and custom uploaded images are all self-made images. Platform administrators, tenants, and authorized sub-accounts all have the right to view and manage them.

Self-made images and ISO images can be used to create virtual machines, and support users to download virtual machine images to their local, while image management supports viewing images, modifying names and remarks, creating virtual machines from images, importing images, downloading images and deleting images, etc. for life cycle management.

## 4.1 View the image

Go to the virtual machine console through the navigation bar, switch to the image management page to view the list of self-made image resources and ISO image resources under the current account, as well as relevant detailed information, including image name, resource ID, status, encryption, feature support, system type, operating system and operation items, as shown in the following figure:

![selfimage1](/assets/images/userguide/selfimage11.png)

- Mirror Name/Resource ID: The name of the current self-made mirror and the global unique ID identifier.
- Status: The status of the current self-made image, including making, uploading, available, failed, deleting, and deleted.
  - In progress: The status of the image is in progress during the process of self-made image through virtual machine.
  - Uploading: During the process of importing the image through the import image function, the status of the image is uploading, and the upload supports viewing the progress.
  - Failure: refers to the user's failure to upload the image.
  - Available: refers to the current image being in an available state, which can be used to create virtual machines or be downloaded.
  - Deleting: refers to the current image being deleted.
  - Deleted: The current image has been deleted and is in the recycle bin.
- Is encrypted: indicates the encryption status of the current image.
- Feature support: refers to the feature support items of the current self-made image, such as hot upgrade, cloud-init, qemu-ga.
- System Type: Represents the operating system type of the current self-made image, such as Linux, Windows, Kylin, etc.
- Operating system: refers to the current self-made image of the basic operating system release, such as CentOS 7.4 x86_64;
- Project Group: Information about the current self-made image project group.
- Operation: Operations on individual self-made images, including creating virtual machines from images, downloading images, interrupting uploads and deleting images.

To facilitate tenants to maintain and operate the image resources, the platform supports downloading the list information of all self-made image resources owned by the current user as an Excel table, and also supports batch deletion operations for self-made images. Multiple self-made images can be selected and the batch deletion button can be clicked to perform batch operations.

The platform supports users to import custom images, and provides users with self-made image documents on the list. By viewing the documents, users can read the steps of making images and format conversion, which facilitates the import of images and business migration.

## 4.2 Create a virtual machine from the image.

Creating a host from an image refers to recreating a virtual machine from a self-made or custom imported image, with the virtual machine starting with the self-made image, and the programs and data in the virtual machine remaining in the same state as when the self-made image was created.

Users can create a virtual machine through the "Create Virtual Machine" operation item in the resource list of the mirror management. After clicking, it will jump to the virtual machine creation interface, as shown in the following figure. The virtual machine creation interface will display the image version currently possessed by the platform according to the user-selected self-made image or ISO image.

![selfimage](/assets/images/userguide/selfimage2.png)

The process of creating a virtual machine using self-made images and ISO images is the same as that of the basic image, and you can operate according to the prompts. When creating a virtual machine from an image, the administrator password set will override the password in the original image operating system, and you need to use the new password to log in to the created virtual machine.

> Creating a virtual machine from an encrypted image does not currently support modifying the key.

## 4.3 Import the image

Importing images refers to tenants or platform administrators migrating third-party business virtual machines to the platform image repository in the form of images, so that tenants can create and deploy business virtual machines through imported images, which is an important channel for users to migrate their business.

Support users to import Linux and Windows distributions and custom images, and support X86 and aarch64 two system architectures images import; The image format of the cloud platform is RAW by default. Upload ISO format images in the ISO image management interface. When users upload VHD, VMDK, QCOW2, OVA, etc. format images, they need to convert the images to QCOW2 format images before importing. For specific operations on converting images and custom images, please refer to the [Custom Image Guide](/CloudDocs_v2.x/customimage/README.md) displayed in the self-made image list . 

After the user has made the custom image, they can go to the "Import Image" function at the top of the Image Management Console resource list to enter the Import Image Wizard page.

![importimage](/assets/images/userguide/importimage_1.png)

![importimage](/assets/images/userguide/importimage_url.png)

* Mirror Name/Description: The name and related description information of the mirror.
* Import method: users can choose to import QCOW2 and ISO format image files from local files or URLs.
* Mirror file: When selecting the import method as local file, you can select the local file selected by the current browser for uploading.
* Mirror address: When the import method is URL, the platform reads and downloads the URL address of the mirror when importing the mirror, and the URL address must be provided when importing the mirror. The platform will automatically download the mirror from the provided URL address and automatically import it into the mirror warehouse for creating virtual machines.
  * Currently only URL addresses with protocols such as HTTP, HTTPS are supported, the format includes `https://path/file` or `ftp://hostname[:port]/path/file` or `ftp://user:password@hostname[:port]/path/file`;
  * The address of the mirror must be accessible from the cloud platform, that is, the URL address accessible by the cloud platform component, it is recommended to use the same external network IP address of the cloud platform or the external network IP address that can communicate.
* Operating System: The type of operating system for importing images, including Linux and Windows, needs to be selected according to the imported image OS type.
* System Architecture: System architecture for importing images, including `x86_64` and `aarch64`, needs to be selected according to the imported image.
* Guidance mode: Boot mode for importing images, supporting `BIOS` and `UEFI`.
* System platform: the operating system platform that guides the import of images.
  * The system platforms of the Linux operating system include Centos and Ubuntu.
  * The system platform of the Windows operating system only supports Windows.
* System version: The operating system version to be imported currently.
  * CentOS x86_64 architecture supports versions `6.5~6.10` and `7.0~7.9` .
  * CentOS aarch64 architecture supports versions `7.6~7.9` .
  * The Ubuntu x86_64 architecture supports versions `14.04` and `20.10` .
  * Ubuntu aarch64 architecture supports versions `16.04` and `18.04`;
  * Windows server supports versions from `2008` to `2019` .
  * Windows supports versions `7` to `10` .
* Agent support: Only appears when importing self-made images, the features supported by the currently imported image, including cloud-init, qemu-ga.
* Label: Select and bind tags for images that support import.

After the mirror image is imported, a mirror image with the status of "uploading" is generated in the self-made mirror image list. Due to the platform needs to first download the mirror image to the mirror image warehouse and the mirror image is usually large, the time for importing the mirror image is usually long. During the import process, clicking "Cancel Upload" can interrupt the upload.

When the mirror status is changed to available, it means that the mirror is successfully imported and can be used to create virtual machines or download mirrors. If an unexpected failure occurs during the mirror import process, the mirror status will be changed to "failed" and the failed mirror can be deleted and re-imported.

> Before importing the image, make sure the image address is accessible and readable and can be downloaded to the image.

## 4.4 Download the image.

Downloading an image refers to the user downloading the platform-made image to their local device for backup or migration. Virtual machine images are usually gigabyte-level files, so to ensure the download image has features such as breakpoint continuation, the platform provides a download address to support the image download; it can be downloaded via FTP, SFTP, and related tools to ensure breakpoint continuation and increase the success rate of the image download.

If the user needs to download the image to the local, they can enter the image download wizard page through the "Download" option in the self-made image list operation item, as shown in the following figure:

![downloadimage](/assets/images/userguide/downloadimage2.png)

After clicking to generate the download address, the platform will jump to the download address display wizard page. Through the wizard page, the user can copy the download address link and download the image through HTTP, FTP and related download tools.

![downloadimage1](/assets/images/userguide/downloadimage3.png)

> The validity period of the mirror download address is 24 hours, and the mirror download needs to be completed within 24 hours. If the mirror download address expires, it cannot be downloaded, and you need to go to the platform to regenerate the mirror download address.

## 4.5 Delete custom images

Users can delete their own custom images, and the deleted custom images will automatically enter the "**Recycle Bin**". Users can restore or destroy them. Users can delete custom images through the "Delete" function in the custom image management console. After deletion, users can view the deleted custom images in the Recycle Bin.

![rmimage](/assets/images/userguide/rmimage1.png)

Only custom images with a status of available or import failed can be deleted; if a virtual machine has been created using a custom image, the custom image cannot be deleted until the virtual machine is deleted.

## 4.6 Edit name and remarks

You can modify the name and remarks of the custom image at any state. You can modify it by clicking the "Edit" button on the right side of each image name on the custom image list page.
