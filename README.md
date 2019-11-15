![Image](https://xebialabs.com/wp-content/uploads/files/tool-chest/cloudformation.jpg)

# CloudFormation Basics Project

## 1. Introduction
This project was built as a way for me to learn cloudformation and to apply certain concepts learned in AWS in cloudformation. The cloudformation stack creates a VPC, an ALB, an ASG, a small HTTP server and creates all the neccasary routes and SGs for the application to work. CloudFormation stack will then output a ALB DNS Name which can be used in a browser to check that the Webpage is online.


## 2. Purpose
The purpose was to learn about how to create a cloudformation stack and also be able to apply them to create a basic template that I could reuse if ever needed. This basic template can be explanded upon and can be used to create larger projects. Hopefully this can also be used to help people learn abit about how cloudformation works.

## 3. Project and Concepts Applied 
In this section I will be going over the template from Top to Bottom and giving Links of documentation that should help you with understanding the different concepts. It should help you learn how to the documentation can be applied in a projects. **This Project is a barebones project and does NOT go over all documentation but the purpose is more to see how the documentation can be applied to create a CloudFormation template.** I will be going over the concepts applied from top to bottom. Meaning that I will be going over the file [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml "ASG_And_ALB.yaml") and I write about different topics that are applied from the top of the file to the bottom of the file. **IMPORTANT** is that when I reference the file [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml) I will always reference specific lines  when I'm taking about a speific  topic. Lets Begin:

* Parameters:
[Parameters]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html)) are used for as values you want to use in you cloudFormation stack. For example in [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L19-L27) you can see the parameters called number of AZs. This parameters you can pick the amount of avaliablity Zones you would want to deploy your application in which would later help with creating subnets. 

* Mappings:
[Mappings]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html] (https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html)) are used for Key value stores. It can be used for storing certain values based on region which cannot be found through the documentation for example specific AMI IDs in specific regions. In For example in [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L63-L100) this is something that can be observed and is used for getting the right AMI based on the region for the Launch Configuration.

* Conditions:
[Conditions]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/conditions-section-structure.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/conditions-section-structure.html)) can be used for conditional creation of certain resources. The conditions section in the code can be used for conditions are often used in the template and where in could save time and code to just create a reference to the condition instead. In For example in [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L109-L112) this is used to decide whether to build a piblic subnet or not. You might ask why do you not just less than ( < ) sign. Well CloudFormation does not have that so we have to evaluate using [Condition Intrinsic Functions]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html#intrinsic-function-reference-conditions-and](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html#intrinsic-function-reference-conditions)). 


* Resources:
In the resources section you actually start to define which resouces you want in you cloudFormation Stack.

* VPC, IGWs and Attach IGW
Once you understand the structure of a resource it is relativly simple creating an [VPC]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html)) , [IGW]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-internetgateway.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-internetgateway.html)) and [attaching the IGWs]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc-gateway-attachment.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc-gateway-attachment.html)). In the [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L135-L136) you might notice something like [!Ref]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html)). Ref is like a intrinsic function and helps with refering to different different resources. In [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L135-L136)  you can see it being used to refer to the VPC and the InternetGateway so that the cloudFormation understand which VPC and which IGW it should use.


* Subnets
Subnets are used as a logical seperation between different resources in a VPC. It can also be used to attach certain resouces to different Avaliability Zones. In [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L143-L180) you might notice that I name them as public subnets. This is because the user data for the EC2 instances require an internet connection. You might then ask why not use a NAT gateway? Well, that cost money which I do not have. So making them public was just to save cost. To be clear we have not made the subnets public currently we have just created them. The Subnets also have a condition. This is important since the condition section is used to determine whether to not to create the resource.  [!GetAZs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getavailabilityzones.html)  and [!Select](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-select.html) are intrinsic functions that help with chosing an avaliablity zone for each subnet. We also created a subnet for the ALB.

* Route Table, Route and Route Table Association
* 

## 4. How To Use it?
You can use the AWS console and upload it there. Pick the parameters. Make sure to have a Role for your CloudFormation template. Click Create Stack. 

You can use the CLI aswell but I dont know how LUL.

In the output section of your CloudFormation stack you will be able to see the DNS name for the ALB and you should be able to use to check that you get a webpage.
## 5. Possible Improvements
