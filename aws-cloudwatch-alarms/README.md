# CloudWatch Alarm Parameters

** The [Request Parameters](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricAlarm.html#API_PutMetricAlarm_RequestParameters) section in the Amazon CloudWatch documentation contains the detailed description of each supported AlarmActions.

## Recommended Commonly Used Metrics

** [AWS Service Recommended alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Best_Practice_Recommended_Alarms_AWS_Services.html) 
The following sections list the metrics that we recommend that you set best practice alarms for. For each metric, the dimensions, alarm intent, recommended threshold, threshold justification, and the period length and number of datapoints is also displayed.

Some metrics might appear twice in the list. This happens when different alarms are recommended for different combinations of dimensions of that metric.

### EC2
> Commonly monitored metrics are CPU Utilization and Status Check Failed. 
Please read [AWS’ documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html) for more information on EC2 metrics.

* CPUUtilization: Average -- The percentage of physical CPU time that Amazon EC2 uses to run the EC2 instance, which includes time spent to run both the user code and the Amazon EC2 code

Typically, you can set the threshold for CPU utilization to 70-80%. However, you can adjust this value based on your acceptable performance level and workload characteristics. For some systems, consistently high CPU utilization may be normal and not indicate a problem, while for others, it may be cause of concern. Analyze historical CPU utilization data to identify the usage, find what CPU utilization is acceptable for your system, and set the threshold accordingly.

* StatusCheckFailed: Maximum -- Reports whether the instance has passed both the instance status check and the system status check in the last minute

This alarm is used to detect the underlying problems with instances, including both system status check failures and instance status check failures
When a status check fails, the value of this metric is 1. The threshold is set so that whenever the status check fails, the alarm is in ALARM state.

** For more information related to EC2, see [Enable or turn off detailed monitoring for your instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-cloudwatch-new.html).

### ECS
> Commonly monitored ECS cluster metrics Disk Utilization. For entire ECS service, alarm recommendation is: CPUUtilization: Average

** ECS/Container Insights Alarm recommendations: **
-----------------------------------------------
* RunningTaskCount: Average -- The number of tasks currently in the RUNNING state.

This alarm is used to detect whether the number of running tasks are too low. A consistent low running task count can indicate ECS service deployment or performance issues.
You can adjust the threshold based on the minimum running task count of the ECS service. If the running task count is 0, the ECS service will be unavailable.

* EphemeralStorageUtilized: Average -- The number of bytes used from ephemeral storage in the resource that is specified by the dimensions that you're using. 

Ephemeral storage is used for the container root filesystem and any bind mount host volumes defined in the container image and task definition. The amount of ephemeral storage can’t be changed in a running task. This metric is only available for tasks that run on Fargate Linux platform version 1.4.0 or later

This alarm is used to detect high ephemeral storage usage for the Fargate cluster. Consistent high ephemeral storage utilized can indicate that the disk is full and it might lead to failure of the container.

Set the threshold to about 90% of the ephemeral storage size. You can adjust this value based on your acceptable ephemeral storage utilization of the Fargate cluster. For some systems, a consistently high ephemeral storage utilized might be normal, while for others, it might lead to failure of the container.

### EKS
> Commonly monitored metrics are cluster_failed_node_count and node_cpu_utilization
Please read [AWS’ documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-metrics-EKS.html) for more information on EKS metrics.

### Elastic BeanStalk

* TDB

### SNS
> Commonly monitored metrics are NumberOfNotificationsDelivered and NumberOfNotificationsFailed

* NumberOfNotificationsDelivered: Sum -- The number of messages successfully delivered from your Amazon SNS topics to subscribing endpoints

For a delivery attempt to succeed, the endpoint's subscription must accept the message. A subscription accepts a message if a: it lacks a filter policy or b: its filter policy includes attributes that match those assigned to the message. If the subscription rejects the message, the delivery attempt isn't counted for this metric.

This alarm helps detect a drop in the volume of messages delivered. You should create this alarm if you expect your system to have a minimum traffic that it is serving.

* NumberOfNotificationsFailed: Sum -- The number of messages that Amazon SNS failed to deliver

For Amazon SQS, email, SMS, or mobile push endpoints, the metric increments by 1 when Amazon SNS stops attempting message deliveries. For HTTP or HTTPS endpoints, the metric includes every failed delivery attempt, including retries that follow the initial attempt. For all other endpoints, the count increases by 1 when the message fails to deliver (regardless of the number of attempts). This metric does not include messages that were rejected by subscription filter policies. You can control the number of retries for HTTP endpoints. For more information, see [Amazon SNS message delivery retries](https://docs.aws.amazon.com/sns/latest/dg/sns-message-delivery-retries.html).

This alarm helps proactively find issues with the delivery of notifications and take appropriate actions to address them.