# AWS_CloudWatch_CloudTrail_Lab
AWS_CloudWatch_CloudTrail_Lab

Step 1: Create an EC2 instance and configure CloudWatch for monitoring resource performance (30 minutes)
  Login to your AWS Management Console
  Click on EC2 from the list of AWS Services
  Click on Launch Instance
  Choose an Amazon Linux AMI and click Select
  Choose t2.micro and click Next: Configure Instance Details
  Keep all the default settings and click Next: Add Storage
  Keep all the default settings and click Next: Add Tags
  Add a tag with Key "Name" and Value "CloudWatch EC2 Instance" and click Next: Configure Security Group
  Choose Create a new security group and add a rule for HTTP traffic and click Review and Launch
  Click Launch and choose an existing key pair or create a new one and download the key pair
  Click Launch Instances and wait for the instance to become available
  Connect to the instance using the key pair and run the following commands to install CloudWatch agent:
    sudo yum install -y awslogs
    sudo systemctl start awslogsd
  Configure CloudWatch agent to monitor CPU utilization and disk usage by running the following command:
    sudo nano /etc/awslogs/awslogs.conf
  Add the following content to the file and save it:
<
[general]
state_file = /var/awslogs/state/agent-state

[mon-cpu-utilization]
datetime_format = %b %d %H:%M:%S
file = /var/log/cloudwatch/cpu_utilization.log
buffer_duration = 5000
log_stream_name = {instance_id}
initial_position = start_of_file
log_group_name = /var/log/cloudwatch/cpu_utilization.log

[mon-disk-usage]
datetime_format = %b %d %H:%M:%S
file = /var/log/cloudwatch/disk_usage.log
buffer_duration = 5000
log_stream_name = {instance_id}
initial_position = start_of_file
log_group_name = /var/log/cloudwatch/disk_usage.log
>
  Restart the CloudWatch agent by running the following command:
    sudo systemctl restart awslogsd
  Verify that the monitoring data is being sent to CloudWatch by navigating to CloudWatch in the AWS Management Console and checking the metrics for the instance.

Step 2: Set up CloudWatch Alarms for high CPU usage and low free storage (15 minutes)
  Navigate to CloudWatch in the AWS Management Console
  Click on Alarms in the navigation menu
  Click on Create Alarm
  Choose EC2 from the list of metrics
  Choose Per-Instance Metrics
  Select the instance created in step 1 and choose the CPU Utilization metric
  Set the alarm to trigger when CPU utilization is greater than 90% for 5 consecutive data points
  Choose a SNS topic or create a new one to send notifications when the alarm is triggered
  Give the alarm a name and description and click Create Alarm
  Repeat the steps to create an alarm for low free storage on the instance
  
 Step 3: Set up S3 event to trigger CloudWatch Logs
 
  Go to the S3 service in the AWS Console
  Create a new S3 bucket and enable bucket versioning
  Create a new S3 event notification that triggers CloudWatch Logs
  Choose the newly created bucket and select the event type (e.g. object creation) that will trigger the CloudWatch Logs
  In the CloudWatch Logs console, select the appropriate log group and choose to create a new metric filter
  Use a filter pattern to extract meaningful data from the logs (e.g. number of 404 errors)
  Create a new metric and give it a name, choose the namespace, and the dimension.
  
Step 4: Set up S3 event to trigger CloudWatch Logs

  Go to S3 service, create a new bucket
  Enable bucket versioning and create a new S3 event notification to trigger CloudWatch logs
  Choose the bucket and event type (e.g. Object creation) to trigger the CloudWatch logs
  In the CloudWatch Logs console, select the appropriate log group and choose to create a new metric filter
  Use a filter pattern to extract meaningful data from the logs (e.g. number of 404 errors)
  Create a new metric and give it a name, choose the namespace, and the dimension
  
Step 5: Create CloudWatch Alarms

  Go to the CloudWatch Alarms service and create a new alarm
  Choose the metric created in the previous step, set the alarm threshold (e.g. if 404 errors are more than 10 in 5 minutes)
  Choose the actions to take when the alarm is triggered (e.g. send an SNS message)
  Review and create the alarm
  
Step 6: Create a CloudTrail Trail

  Go to the CloudTrail service and create a new trail
  Choose the S3 bucket to store the logs, enable log file validation
  Optionally, you can also choose to send events to CloudWatch Logs and create an SNS topic for notifications
  Review and create the trail
  
Step 7: Monitor and analyze CloudTrail and CloudWatch Logs

  Use the CloudTrail and CloudWatch Logs console to monitor and analyze the logs
  Filter and search the logs to see specific events and metrics
  Use the CloudWatch Alarms to be notified of any events that exceed the threshold
