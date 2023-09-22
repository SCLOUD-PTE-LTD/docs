---
layout: default
title: Notifications Group
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/notifications-group/
nav_order: 30
---
# 30. Notifications Group

A notification group refers to the way of sending alarm notifications and contact information, by recording user emails, webhook addresses, and sending alarm notifications of different resources through the above two methods to notify people, in order to divide responsibility and refine alarm notifications.

- A notification group is a combination of notification people, which can include one or more contacts.
- The same contact can be added to multiple notification groups.
- The notification methods include email notification and webhook notification.

When using a monitoring alert template, you need to first create a notification group, add relevant contact information, and set the notification method of the notification group in order to associate the alert template. For specific management of the notification group, see the following.

### 30.1 Create a notification group

Users can enter the **Operation and Management** module through the left navigation bar of the console, switch to the **Notification Group** page, and enter the notification group creation wizard page through the "Create Notification Group" button on the notification group management page, as shown in the following figure, specify the notification group name and notification method to create the operation:

![createnotice](/assets/images/userguide/createnotice.png)

- Notification Group Name: Name and identifier of the notification group to be created currently.

After clicking confirm, you will enter the notification group list page, where you can view the information of the created notification group and manage it.

### 30.2 View the notification group

Users can enter the notification group page through the monitoring alarm control console to view the notification group list information, and can also enter the detail page by clicking the name of the notification group on the list, which is used to view the detailed information of the notification group and manage the notifier.

#### 30.2.1 Notice of Group List

The notification group list page can view the list of notification groups currently owned by the account, including the name, ID and operation items of each notification group, as shown in the following figure:

![noticelist](/assets/images/userguide/noticelist.png)

- Name/ID: Name identifier of notification group and globally unique identifier.
- Notification Method: Notification method of the current notification group.
- Operation: Operation items for individual notification groups, including details, updates, deletions, etc.

#### 30.2.2 Notification Group Details

By entering the notification group detail page through the `ID` of the notification group list, you can view the basic information of the current basic notification, as shown in the following figure:

![noticedetails](/assets/images/userguide/noticedetails.png)

- Basic notification information: including notification person name, notification person email and create, update, delete operations;

Press the Webhook button to enter the webhook notification person details page, where you can view the basic information of the current webhook notification and manage the notification contacts through the notification management, as shown in the following figure:


![noticewebhook](/assets/images/userguide/noticewebhook.png)

- Webhook notification information: including the notification person's name, request method, request address, creation time, update time, and create, update, delete operations.
- Notification Person Management: Current notification group notification contact management, including notification person creation, viewing, updating and deleting, see [Notification Person Management](#Notification Person Management).

### 30.3 Update Notification Group

Updating a notification group refers to modifying an individual notification group. The selection and configuration of modification items is the same as creating a notification group, please refer to [Creating Notifiers](#Creating Notifiers).

### 30.4 Delete Notification Group

Before deleting a notification group, it must be confirmed that the notification group is not bound to any alarm rule. If it has been added to an alarm rule, it cannot be deleted. The notification group that has been successfully deleted will be destroyed, and the user must confirm it before it can be successfully deleted. Users can delete the notification group through the "Delete" operation item in the notification group console list, as shown in the following figure:

![noticerm](/assets/images/userguide/noticerm.png)

### 30.5 Notice Person Management

A notification person refers to the specific contact person who sends the notification for the alarm rule, including contact information such as the contact person's name, email or webhook address, etc. Each notification group can add one or more notification persons, and when the resource triggers an alarm, it will be sent to all notification persons through the set notification method.

#### 30.5.1 Create Notifier

Users can add notification recipients through the "**Create**" function on the notification group basic notification page and webhook page. When creating notification recipients, different notification parameters need to be added according to different notification methods, as shown in the following figure:

asic Notification Creation Illustration

![contact](/assets/images/userguide/contact.png)

- Notification Name: The name or nickname of the contact to be created currently.
- Notification Email: refers to the email address of the contact to be created currently.

Webhook Notification Creation Illustration

![contactwebhook](/assets/images/userguide/contactwebhook.png)

- Notification Name: The name or nickname of the contact to be created currently.
- Request Method: The request method for the warning message sent, either GET or POST (GET request method is not supported by DingTalk).
- Request address: Obtain from the robot settings of the corresponding product, and refer to the following documents for specific settings.

DingTalk https://open.dingtalk.com/document/robots/custom-robot-access
Feishu https://www.feishu.cn/hc/zh-CN/articles/360024984973

After clicking confirm, a notification contact will be successfully created. You can view the contact information through the notification person list in the notification group details.

#### 30.5.2 pdate notification contact information

Updating notification person information refers to modifying the information of an individual notification person. The configuration of the modification items is the same as the rule for creating a notification person, and can refer to [Creating a Notification Person](#Creating a Notification Person).

#### 30.5.3 Delete Notifier

Delete notification person refers to deleting a single notification person, after which the notification person is directly destroyed and contact information can be re-added.


