---
layout: default
title: Scheduler
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/scheduler/
nav_order: 16
---
# 16. Scheduler

## 16.1 Product Introduction

A Scheduler is a platform provided by the user to automate tasks, which can be used to regularly execute a series of tasks, such as creating snapshots. It can be repeated on a specified cycle, or only executed once, and each task supports batch operations on multiple resources.

* Support for scheduled snapshots, that is, automatic snapshots of disks, and support for batch creation of scheduled snapshot tasks for multiple disks.
* Support single and repeated scheduled tasks, repeated execution supports specified time execution tasks every day, every week, and every month.
* Support for executing scheduled tasks on an hourly basis or every hour daily.
* Support for one hour per week from Monday to Sunday or hourly for scheduled task execution.
* Support the execution of scheduled tasks every day for one hour or every hour per month.
* The platform will save the task list and execution results of the timer, and support users to view the execution records of each task in the timer.

The platform supports the full life cycle management of timers, including creating timer tasks, viewing timer tasks and records, updating timer tasks, and deleting timer tasks.

## 16.2 Create a scheduled task

Users can directly create a scheduled task for creating snapshots for cloud disk backup through the console. When creating the scheduled task, the task type, repeat cycle, execution time and associated resources need to be specified, as shown in the following figure:

![createjob](/assets/images/userguide/createjob.png)

* Name: Name of the scheduled task, used to identify the scheduled task.
* Task Type: Execution action of scheduled task, currently only supports creating snapshot.
* Repeat cycle: Support single and repeat execution of scheduled tasks, repeat execution supports specified time execution of tasks every day, every week, and every month.
  * Single execution: Specific execution time needs to be specified in 24-hour format; Default execution on the same day, if the execution time has passed then it will be executed the next day.
  * Every day execution: Need to specify the time of every day execution, support 24 whole points from 0 to 23, support that every whole point will execute the timed task.
  * Perform weekly: The date and time of each execution need to be specified, supporting from 0 o'clock to 23 o'clock from Monday to Sunday, and supporting the execution of timed tasks every day from Monday to Sunday at each execution time.
  * Monthly execution: Need to specify the date and time of each execution, support from 1st to 31st or the end of the month from 0:00 to 23:00, and the execution time of each day of the month for timed task execution.
* Execution time: The specific execution time of the task, depending on the different repetition cycles, the execution time can support different settings.
  * When the repeat cycle is set to single execution, it supports setting the exact date, hour, minute, and second.
  * When the repeating cycle is executed daily, weekly, or monthly, it can support every whole hour from 0 to 23.
* Time zone: The default time zone for scheduled tasks is Eastern Eight Zone time, i.e. GMT+8 (Beijing time).
* Associate resources: that is, set up cloud disk resources that need to be scheduled for snapshot creation, support system disks and data disks of virtual machines, and support batch selection.

After clicking confirm, the system will automatically generate a scheduled task and immediately check the execution plan and time of the scheduled task. If it reaches the agreed time, the scheduled task will be executed immediately. The specific execution situation can be viewed in the log of the scheduled task details.

## 16.3 View Scheduled Tasks

### 16.3.1 Scheduled Tasks List

Users can view a list of all scheduled tasks and related information in their account through the console, and can enter the scheduled task details to view the basic information and task execution records of the scheduled task by the name of the list. The specific list information is shown in the following figure:

![joblist](/assets/images/userguide/joblist.png)

The list information includes name, ID, task name, timer, associated resources, creation time, update time and operation items.

* Name/ID: The name and globally unique identifier of the current scheduled task.
* Task Name: Execution Action of Current Timed Task, Currently Only Snapshot Creation is Supported.
* Timer: The execution rules of the timed task, including the repetition cycle, execution date and execution time, where the execution date is only valid when repeating the execution.
* Associated resources: Associated resources when executing scheduled tasks, that is, associated resources executing creating snapshots or other operations.
* Creation/Update Time: Creation and update time of scheduled tasks.

The list supports updating and deleting operations for scheduled tasks, and also supports batch deleting operations for scheduled tasks; in order to facilitate users to manage the resources of scheduled tasks, the platform supports searching for scheduled tasks, and supports fuzzy searching.

### 16.3.2 Scheduled Tasks Details

Users can enter the Timed Task Details to view the detailed information of the Timed Task, including basic information and task execution record information, as shown in the following figure:

![jobdetails](/assets/images/userguide/jobdetails.png)

The basic information includes name, ID, task type, repeat period, execution time, associated resources and creation time, while the task execution log comprehensively records the task execution plan and execution status of the current timed task, including resource ID, start time, end time, task status and task result.

* Resource ID: The associated resource corresponding to the execution record of the scheduled task.
* Start time: the start time of the scheduled task execution.
* End time: The end time of the scheduled task execution.
* Task Status: The current status of the scheduled task, including success and failure.
* Task result: Final resource operation status of the current task, including successful operation and operation failure.

By tracking the execution records, it can be used to determine the success or failure of the scheduled task.

## 16.4 Update scheduled tasks

Users can modify the scheduled tasks when needed, supporting the modification of task types, repeat cycles, execution dates, execution times, and associated resources, as shown in the following figure:

![updatejob](/assets/images/userguide/updatejob.png)

After the scheduled task is updated, it will not affect the tasks that have already been executed. When executing new task types, it will operate according to the new scheduled policy.

## 16.5 Delete the scheduled task

The platform supports users to delete scheduled tasks that are not suitable for business scenarios, which can be deleted in any state. As shown in the following figure:

![rmjob](/assets/images/userguide/rmjob.png)

Deleting a scheduled task will immediately destroy it, and automatically stop the execution plan in the scheduled task, but will not affect the resources associated with it or created by it.

