---
title: Pausing the EC2 instances initialized through Elastic Beanstalk
date: '2019-10-23 11:55'
template: "post"
draft: false
slug: "/posts/pausing-the-ec2-instances-initialized-through-elastic-beanstalkc/"
category: "AWS"
tags:
  - "AWS"
description:
socialImage: "/media/image-2.jpg"
---
* EC2 free tier provides you with **_monthly_** 750 hours usage of Linux (any combination of t2.micro and t1.micro instances), plus 750 hours usage of Windows (any combination of t2.micro and t1.micro instances)
* So if you have couple of applications or microservices deployed on separate EC2s, then you will run out of the free tier limits of the month.
 (24 hours \* 30 days = 720 hours per EC2 instance).
So if you have 2 EC2 instance, then - 720 hours \* 2 = 1440 hours :(
* And if you have deployed your application using Elastic Beanstalk service, then your trouble increase because there is no simple 'pause' and 'resume' actions available.

## Solution - use Time-based scaling
* One of the solution is to use time based scaling to pause and resume the EC2 capacities.
* Using time-based scaling, you can schedule a task to turn your EB environment to have 0 instances running and thus pausing your environment.

### Steps to pause an EB environment:
1. On AWS Elastic Beanstalk console, select the `environment` you want to pause.
2. On left panel, select `Configuration`.
3. On Configuration Overview menu, modify `Capacity`
4. On Modify Capacity menu, scroll down to `Time-based scaling`
![time-based-scaling](/img/aws-elastic-beanstalk-console-time-based-scaling.png)
5. Select `Add scheduled action` .
6. Set the Min and Max of instances and Desired capacity to 0.
7. Set the start time about 5 minutes from your current UTC time so that the pause action has enough time to execute.
Note that it is in UTC(Coordinated Universal Time).
8. Click `Add` to close the action menu.
9. Choose Local for time zone and check if scaling is scheduled at the right time.
10. Click `Apply` so that this added scheduled action will take effect.
11. Your environment will be updated and will be set to 0 instance.


### Steps to resume an EB environment
1. Now that you have your EB environment paused, when you want your EB instance to resume running, you can repeat the steps above but this time, you should set min, max of instances and desired capacity to the number that you would like it to be.
2. Your scheduled actions are saved in time-based scaling action list. Thus, subsequently, you just need to reschedule the needed action.

Reference - [https://jun711.github.io/aws/how-to-pause-or-stop-elastic-beanstalk-environment-from-running/]()
