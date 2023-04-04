---
layout: default
title: Self-managed Image
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/self-managed-image/
nav_order: 3
---
# Self-managed Image
Self-managed images are attributed to the cloud platform tenant, and self-managed images exported from virtual machines and images imported by custom uploads belong to self-managed images, and platform administrators, tenants, and authorized sub-accounts have permission to view and manage them.

Custom images can be used to create virtual machines, and enable users to download virtual machine images to the local computer, while image management supports lifecycle management such as viewing images, modifying names and comments, creating virtual machines from images, importing images, downloading images, and deleting images.

## View self-managed images
Go to the VM console through the navigation pane and switch to the Image Management page to view the list of self-managed image resources under the current account and related details, including image name, resource ID, system type, operating system, status, and operation items, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-43.png)

- Image Name/Resource ID: The name of the current self-managed image and the globally unique ID identifier;
- System type: represents the operating system type of the current self-managed image, such as `Linux, Windows, Kylin`, etc.;
- Operating system: refers to the base operating system distribution of the current self-managed image, such as `CentOS 7.4 x86_64`;
- Status: The status of the current self-managed image, including Creating, Importing, Failing, Available, Deleted, and Deleted.
  - Creating: During the process of self-creating an image through a virtual machine, the status of the image is Creating;
  - Importing: During the process of importing an image through the Import Image feature, the status of the image is Importing;
  - Import failure: This means that the user failed to import the image.
  - Available: The current image is available and can be created or downloaded.
  - Delete: refers to the current image being deleted;
  - Deleted: The current mirror image has been deleted and entered the Recycle Bin.
- Operations: Operations on a single custom image, including creating a virtual machine from the image, downloading the image, and deleting the image;

In order to facilitate tenants to maintain and operate image resources, the platform supports downloading the list of all self-managed image resources owned by the current user as an Excel table, and supports batch deletion of self-managed images.

The platform supports users to import custom images, and users on the list can create image documents by themselves, and read the operation steps of image creation and format conversion by viewing the documents, which is convenient for image import and business migration.

## Create a virtual machine from an image
Creating a host from an image means re-creating a virtual machine from a custom or custom-imported image, starting the created virtual machine with the self-managed image, and the programs and data in the virtual machine remain in the state when the self-managed image was created.

Users can create a custom image through the operation item "Create host from image" in the image management resource list, as shown in the following figure, the virtual machine creation wizard automatically selects a specified custom image for the user.

 ![1](/assets/images/user-guide/user-guide-4.png)

The process for creating a virtual machine using a self-managed image is the same as for a base image, and you can follow the prompts. The administrator password set when creating a virtual machine from the image overrides the password in the operating system of the original image, and you need to log in to the created virtual machine with the new password.

## Import images
Importing an image means that a tenant or platform administrator migrates a third-party service VM to a platform image repository, so that tenants can create and deploy service VMs through the imported image, which is an important channel for users to migrate services.

Support users to import Linux and Windows distributions and custom images, and support the import of X86 architecture and aarch64 system architecture images; The default image format of the cloud platform is RAW, when you upload an image in VHD, VMDK, QCOW2, OVA, ISO and other formats, you need to convert the image to QCOW2 format image before importing.

After you create a custom image, you can use the Import Image feature at the top of the resource list of the Image Management console to enter the Import Image Wizard page:

 ![1](/assets/images/user-guide/user-guide-44.png)

- Image Name/Description: The name of the image and related description information;
- Import method: Users can choose a local file or URL to import an image file in QCOW2 format;
- Image file: If you select the import method as a local file, you can select the local file selected by the current browser to upload it.
- Image address: When URL is selected, the platform reads and downloads the URL address of the image when importing the image, and the platform automatically downloads the image from the provided URL address and automatically imports it to the image repository to create a virtual machine.
  - Currently, only HTTP and HTTPS protocol URL addresses are supported, including `https://path/file` or `ftp://hostname[:port]/path/file` or `ftp://user:password@hostname[:port]/path/file`;
  - The address of the image must be reachable from the cloud platform, that is, the URL address accessible to the cloud platform components, and it is recommended to use the same external IP address of the cloud platform or the address that the external IP address can communicate with.
- Operating system: The operating system type of the imported image, including Linux and Windows, must be selected according to the OS type of the imported image;
- System architecture: The system architecture of the imported image, including `x86_64` and `aarch64`, must be selected according to the imported image;
- System platform: the operating system platform that guides the image;
  - System platforms for Linux operating systems include Centos and Ubuntu;
  - The system platform of the Windows operating system supports Windows only;
- System version: the operating system version to be imported into the image;
  - CentOS `x86_64` architecture supports versions `6.5~6.10 and 7.0~7.9`;
  - CentOS `aarch64` architecture supports version `7.6~7.9`;
  - The Ubuntu `x86_64` architecture supports versions `14.04 and 16.04`;
  - The Ubuntu `aarch64` architecture supports versions `16.04 and 18.04`;
  - Windows supports versions `2008, 2008R2, 2012, 2012R2, and 2016`;
- Target image size: The target size of the currently imported image, the minimum size is 20 GB, and the maximum cannot exceed 500 GB;
- SHA256 : The value used to verify the integrity of the file, which does not need to be specified by default.

After the image is imported, the custom image list generates an image with a status of "Importing", because the platform needs to download the image to the image repository first and the image is usually large, and the time to import the image is usually relatively long.

When the image status is converted to Available, the image is imported successfully, and you can create a virtual machine or download an image. If an unexpected failure occurs during the image import process, the status of the image changes to Import Failed, and the failed image can be deleted and re-imported.

Before importing an image, make sure that the image address is accessible and can be read and downloaded to the image.

## Download the image
Downloading an image means that users download the self-managed image of the platform to the local computer for backup or migration. The platform supports image download by providing download addresses for virtual machine images by providing GB-level files, in order to ensure the resumption of download images and other functions. Image download can be performed through `FTP, SFTP` and related tools to ensure the resumable upload function and improve the success rate of image download.

If you need to download an image to your local computer, you can use Download in the custom image list operation item to enter the image download wizard page, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-171.png)

After clicking Generate Download Address, the platform will jump to the download address display wizard page, through which users can download the image through HTTP, FTP and related download tools by copying the download address link.

The image download address is valid for 24 hours, and the image download must be performed within 24 hours. If the image download address expires, the image download cannot be downloaded, and you need to regenerate the image download address on the platform.

## Delete self-managed images
Users can delete the self-managed image, and the deleted self-managed image will automatically enter the "Recycle Bin" for restoration and destruction. Users can delete the self-managed image through the Delete function of the self-managed image management console, and view the deleted self-managed image in the Recycle Bin after deletion.

 ![1](/assets/images/user-guide/user-guide-45.png)

 Only self-managed images with a status of Available or failed to import can be deleted; If you have created a virtual machine from a custom image, you cannot delete the custom image, and you need to delete the virtual machine before you can delete the custom image.

## Modify Name and Remarks
Modify the name and comment of the self-managed image to perform operations in any state. You can modify it by clicking the "Edit" button to the right of each image name on the custom image list page.