---
layout: default
title: UHub
parent: Compute
grand_parent: Public Cloud
permalink: /public-cloud/compute/uhub/
nav_order: 4
---
# The public image library UHub

UHub is a free public image library service launched by SCloud.
UHub allows users to create and manage image libraries freely. The UHub image library is a cross-region architecture, and images that are pushed in one region can be accessed by other regions.

# How-to guide
## Creation and Deletion
### Create an image repository
On the Image Repository UHub page, click Create Image Repository, enter the image repository name and remarks, and create an image repository. If the current project already has an image repository, select Create Image Repository from the Repository Name drop-down list.

The image repository name must be globally unique (cannot coincide with other users or project image repository names), and each project supports up to 8 image repositories.
### Delete the image repository
Click the Delete Image Database button in the upper-right corner of the Image Repository console to delete the image repository, and only empty image repositories can be deleted. If there are still images in the image repository, delete them first.
## Accounts and Permissions
Log in via docker login on a machine with docker (version 1.10 or later) installed.
Log in to the image repository and access the service by domain name:
```
docker login uhub.service.scloud.sg -u user@scloud.sg
```
The login user name is the email address registered on the SCloud platform, and the password is the console login password.

By default, an image repository is non-public and can only be pulled by the master account and sub-accounts that have corresponding permissions to the project where the image repository is located. If you want your images to be pulled by other users on the platform, you can set the external shared image library to public.

You can also use the Internet access switch to disable pushing and pulling repository images over the Internet. However, if your repository permission is to share the image database, you can only disable public network push, and you still have the permission to pull from the public network.

### Instructions for using the standalone password
The independent password is bound according to the login user name, and the user name of logging in to the image repository after setting is still the email address registered on the SCloud platform, and the password is the set independent password.

Changing the standalone password applies to all image repositories and supports login on SCloud's intranet and extranet.
The standalone password is bound to the login username, not to the image repository.
## Push and pull images
### Push images
Step 1: Tag the image locally:
```
docker tag {local imagename} uhub.service.scloud.sg/{image repository created}/{image}:tag
```
Step2: Commit the image to the repository:
```
docker push uhub.service.scloud.sg/{image repository created}/{image}:tag
```
### Pull images
```
docker pull uhub.service.scloud.sg/{image repository created}/{image}:tag
```
## Cross-region experience
The SCloud public image database is a cross-region architecture, and images pushed by nodes in one region can be pulled from nodes in other regions through the intranet.
For example, in Zone A in Singapore, push the image:

In Singapore 2, you can also pull to:

As long as the image push is completed, all regions covered by the internal network in the SCloud platform can be pulled to the pushed image through the internal network.
## Restrictions on use

A maximum of 800 tags are retained for a single image, and after more than 800, tags and related images that have not been pulled within one month will be deleted, and images uploaded earlier will be deleted first.

There is no limit to the image size, but a single-layer image may fail to push more than 5G.

The speed of pulling images from the Internet is limited to 1 MB/s per layer.

Currently, the region where images are pulled from the intranet is not supported in Fujian.
