# Securing a 3-Tier AWS Architecture with NIST-Aligned Controls

## Project Overview
Voyager, a fictional data and technology company, manages over 2.5 billion consumer credit files under strict regulatory oversight. After acquiring a fraud prevention firm, Voyager integrated its Fraud Risk and Loss (FRL) application into the AWS Cloud. Originally designed for limited use, this application now must align with corporate standards based on NIST 800-53 Rev 5.
As the Cloud Security Architect, I implemented these controls via AWS’s Cloud Adoption Framework (CAF), emphasizing Infrastructure Protection, Data Protection, Security Assurance, and Identity and Access Management. The FRL application operates on a scalable, highly available 3-tier AWS architecture deployed using a pre-built CloudFormation template incorporating EC2, RDS, ELB, VPC, and Internet Gateways.

## Core AWS Services Used
- AWS Key Management Service (KMS) for encryption
- AWS Identity and Access Management (IAM) for secure access control
- AWS Systems Manager (SSM) Patch Manager for patch management
- AWS Config with customized compliance rules for configuration management
- Amazon CloudWatch for real-time monitoring and logging
- Amazon Simple Notification Service (SNS) for notifications
- AWS Secrets Manager for secure storage and management of sensitive information
Together, these services established a secure, compliant cloud infrastructure tailored to Voyager’s regulatory needs.

## Enhanced Infrastructure Security and Compliance
### Infrastructure Protection
To strengthen Voyager’s security and resilience, I developed an automated patching strategy for a single EC2 instance using the SSM agent. This strategy ensures regular updates for optimal performance while adhering to security best practices. Patch Manager is configured to run periodically during a three-hour maintenance window.
The setup involves:
1.	Opening AWS Systems Manager and navigating to Patch Manager.
2.	Selecting "Configure Patching" and identifying the specific instance in the "Instances to Patch" section.
3.	Choosing the relevant option from the provided image in the "Patching Schedule" section.
![image](https://github.com/user-attachments/assets/fe302bcb-5c54-48ab-b6be-6284140832ce)
![image](https://github.com/user-attachments/assets/d6e9be6f-d645-4878-83f9-b457750b1218)

Additionally, to implement a detection mechanism for compliance monitoring, AWS Config is used to set up a compliance rule. The steps include:
1.	Navigating to "Rules", clicking "Add Rule", and selecting the ec2-managedinstance-patch-compliance-status-check rule.
2.	Setting the trigger type to "All Changes" for continuous monitoring.
3.	Reviewing the rule, performing re-evaluation by selecting the rule name, navigating to "Actions", and choosing "Re-evaluate".
4.	Confirming compliance by selecting "All" under the "Resources in Scope" drop-down menu, ensuring the status reflects "Compliant".
![image](https://github.com/user-attachments/assets/a4945453-2a58-4b7d-baea-d8399e7017a4)

### Data Protection 
To enhance data security, I tagged Amazon RDS instances with the label "Classification: Restricted". AWS Config was used to enforce this tagging by setting up a required-tags rule, with regular re-evaluations to maintain compliance and improve access control. The process began by:
1.	Opening the Amazon RDS console.
2.	Navigating to the Tags tab and adding the necessary tag.
![image](https://github.com/user-attachments/assets/9b2020e2-3794-424e-a9dc-49e703c5bb2a)

Next, the AWS KMS key policy was updated to restrict specific actions for the KMSAdminRole and DevOpsRole, ensuring that sensitive data was accessible only to authorized users. After saving the changes, the tagging policy was validated through AWS Config by:
1.	Applying the required-tags rule to the RDS DBInstance.
2.	Entering the parameters, reviewing the rule, and re-evaluating compliance to refresh the status.
![image](https://github.com/user-attachments/assets/21bfc519-cdcc-4edb-88e2-22f759ced887)

### Security Assurance 
To reinforce security assurance, I utilized Amazon CloudWatch for continuous monitoring and analysis of operating system logs. By configuring CloudWatch to collect critical system logs, I achieved real-time visibility into system activities, enabling early threat detection.
I also set up CloudWatch alarms to notify the team of suspicious or unauthorized actions, ensuring swift responses to emerging incidents. The setup included:
1.	Creating a metric filter in CloudWatch Logs to capture the event "session opened for user root".
2.	Monitoring the metric within the CloudWatch console after triggering the event by connecting to an EC2 instance.
This proactive monitoring enabled Voyager to detect abnormal activities and take corrective actions promptly.
![image](https://github.com/user-attachments/assets/a10c890c-c2ef-413e-ad1c-0722a8903dec)

### Identity and Access Management
Using the IAM Console, I navigated to the Roles section, searched for the DevOps role, and edited it. In the JSON tab, I modified the policy to restrict the creation of Traffic Mirror sessions by adding the wildcard “ec2:CreateTrafficMirror*”. After reviewing and saving the changes, the DevOps role's privileges were effectively reduced.
To detect active Traffic Mirror sessions, I deployed a custom AWS Config rule using a CloudFormation template. This setup validated the presence of Traffic Mirror Sessions or Targets by configuring a Lambda function and necessary permissions. I verified the new rule in the AWS Config console.
To test the rule:
1.	Accessed the VPC console and navigated to Traffic Mirroring.
2.	Created a new Traffic Mirror target using an EC2 ENI.
3.	Returned to the AWS Config console, selected the new rule, and clicked "Re-evaluate".
Upon refreshing the page, the rule's status changed from Compliant to Noncompliant, confirming that the detection mechanism functioned as intended.
![image](https://github.com/user-attachments/assets/f9b0e696-e7c3-4f3a-a9cb-ce3559119efb)

## Key Takeaways and Personal Reflection
This project provided an excellent opportunity to tackle real-world issues related to the security and scalability of sensitive data environments. It fueled my enthusiasm for cloud computing and cybersecurity while broadening my knowledge of technical implementation and compliance standards.
The primary goal was to create and implement secure, compliant, and thoroughly monitored AWS solutions. Key approaches included using AWS Systems Manager Patch Manager for automated updates, establishing AWS Config rules to maintain compliance, and employing CloudWatch for real-time monitoring and alerts. Data protection was a top priority, achieved by designating the database as “restricted,” applying RDS tagging, and enforcing least privilege access through KMS Customer Managed Keys (CMKs).
To maintain strong security and compliance:
- Operating system logs were collected and monitored.
- CloudWatch Logs, Alarms, and SNS were utilized to generate alerts for critical incidents.
- Logging standards were upheld through Config.
- IAM policies were tightened, with enhanced oversight via Config, CloudTrail, and traffic mirroring detection.
The insights gained have significantly enhanced my capability to design secure, scalable cloud infrastructures and will directly influence future initiatives focused on strengthening enterprise-level security frameworks.

 



