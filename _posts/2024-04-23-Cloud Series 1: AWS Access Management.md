---
layout: post
title: "Cloud Series 1: AWS Access Management(IAM)"
description: "AWS Access Management-how to use it, best practices"
tags: [AWS, IAM]
# link: http://mademistakes.com  
published: false
authors: 'Bhoj Bahadur Karki'
name: 'Bhoj Bahadur Karki'
share: false
---
### We have used:
- AWS IAM Service

### Assumption of audience
It is assumed that the audience have a basic understanding of cloud technology. 

### Topics Covered
In this series I will be going through how to use AWS access management, how to use the services, the advantages and disadvantages and then finally best practices to consider what to do and what not to do. 

Introduction and Background
Creating Users (IAM Users)
Granting Access for User to Billing Services. 
Creating Groups
Assigning Groups to Users
Creating Roles
Creating IAM Policies.
Roles VS Groups VS Policies.

### Introduction and Background
For managing application and data in the cloud we have to understand how cloud works and who are the key cloud providers. There are mainly 3 cloud providers that occupy a large portion of cloud market and they are Amazon, Microsoft and Google. There are other cloud providers as well such as IBM, Oracle, Salesforce, Alibaba, Digital Ocean, etc.  <br /><br />


In this series I will talk about Amazon Web Services(AWS) which hold the highest amount of share in the cloud as of 2024. In this series we will dive deep into many of AWS services and study them.<br /><br />

In this article we will talk about a very important AWS service, Identity and Access Management(IAM). Before starting to use service we will need user accounts to interact with the service. The IAM service is a global service associated with users, roles, permission or policies and groups. 
Users are the entites that are used to access and use the AWS services. Users can be grouped together in one or many groups. Groups are also AWS entites that are used to set permission and limitation to one or more users in that group. The setting of permission is performed through policies. Policies are json objects that defines what permission the using entity(e.g. group) has.<br /><br />

Another entity is Role which makes requests to an AWS service. It can be assigned to either IAM Users, Applications, or AWS Services sucha as EC2, Lambda, etc.<br /><br />

Let's summarize various terminology in this context:
IAM - an AWS service to manage Users, Groups, Policies, and Roles. 
Users -  are people in an organization which maybe grouped together. 
Groups - an entity which defines permission for users in that group. Groups can have one or more users but not other groups. 
Roles - an IAM entity with permissions to make AWS service requests. 
Policies - a Json object which defines the permissions to which this policy is applied.  

### Accessing IAM


### Best Practices 

### Do's 


### Dont's


### Conclusion
