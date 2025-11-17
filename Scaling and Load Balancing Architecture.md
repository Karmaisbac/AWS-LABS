# Scaling and Load Balancing Architecture

The goals of this lab are to:
Create an Amazon Machine Image (AMI) from a running instance 
Create a load balancer 
Create a launch template and an Auto Scaling group
Automatically scale new instances 
Create Amazon CLoudwatch alarms and monitor the performane of the infrastucture 

The initial structure would be this 
<img width="763" height="492" alt="image" src="https://github.com/user-attachments/assets/76115306-fed8-4360-9e15-9b441492b7f5" />

The end product should look like this 
<img width="780" height="461" alt="image" src="https://github.com/user-attachments/assets/d69b0d85-0724-49ee-9c17-99777861ff92" />

## Create an Amazon Machine Image (AMI) from a running instance
Using an existing EC2 instance called Web Server 1. I created an image which is a copy of the configurations of Web Server 1

I created a target group called LabGroup and chose "Instances" as the target type. The VPC, i selected was the Lab VPC.
I didn't register a target as i didn't have a web application yet.

## Create a load balancer 
I created a load balancer named LabELB, specifically an application load balancer. The VPC chosen is the "Lab VPC". 
"Public Subnet 1" was chosen for AZ 1, and "Public Subnet 2" for AZ 2. I selected the "Web Security Group" under the security group configuration while also deselecting the default security group. The default action to forward was set to LabGroup.

## Create a launch template and an Auto Scaling group
I launched a template called "LabConfig" while also ticking "Auto Scaling Guidance". 
I used the AMI i created earlier called "Web Server AMI".
The instance type was t2.micro, while the key pair name was vockey.
The "Web Security Group" was selected, and i enabled CloudWatch Monitoring in the advanced details.

I created an Auto Scaling group called "Lab Auto Scaling Group". The launch template used was "LabConfig". The "Lab VPC" was chosen as the VPC, Private Subnet 1 ,and 2 were used. 
I attached the "LabGroup" load balancer to the Auto Scaling Group while also enabling the group metrics collection within CloudWatch.
The Group size desired capacity was configured to 2, minimum (2), and maximum (6).
Under the Scaling policy, i called it LabScalingPolicy. The metric type is average CPU utilization, and the target value is 60.

A tag called name was created and the value was Lab Instance. 

## Testing
I copied the DNS of the LabELB balancer, pasted it in a new window, and clicked on load test on the website. I switched to the AWS management console. I went to the alarms in the CLoudWatch service and noticed when the average CPU utilization was over 60% for more than 3 minutes. The high alarm changed to in alarm while low alarm changed to ok. 

## Web Server 1 Termination
The web server was used to create the AMI used by the Auto Scaling group. I terminated it by clicking on Web Server 1, instance state, and teminate. 







