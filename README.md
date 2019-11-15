
![Image](https://xebialabs.com/wp-content/uploads/files/tool-chest/cloudformation.jpg)

# CloudFormation Basics Project

## 1. Introduction
This project was built as a way for me to learn cloudformation and to apply certain concepts learned in AWS in cloudformation. The cloudformation stack creates a VPC, an ALB, an ASG, a small HTTP server and creates all the neccasary routes and SGs for the application to work. CloudFormation stack will then output a ALB DNS Name which can be used in a browser to check that the Webpage is online.


## 2. Purpose
The purpose was to learn about how to create a cloudformation stack and also be able to apply them to create a basic template that I could reuse if ever needed. This basic template can be explanded upon and can be used to create larger projects. Hopefully this can also be used to help people learn abit about how cloudformation works.

## 3. Project and Concepts Applied 
In this section I will be going over the template from Top to Bottom and giving Links of documentation that should help you with understanding the different concepts. It should help you learn how to the documentation can be applied in a projects. **This Project is a barebones project and does NOT go over all documentation but the purpose is more to see how the documentation can be applied to create a CloudFormation template.** I will be going over the concepts applied from top to bottom. Meaning that I will be going over the file [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml "ASG_And_ALB.yaml") and I write about different topics that are applied from the top of the file to the bottom of the file. **IMPORTANT** is that when I reference the file [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml) I will always reference specific lines  when I'm taking about a speific  topic. Lets Begin:

### 3.1 Parameters:
[Parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html) are used for as values you want to use in you cloudFormation stack. For example on [Line 19-27](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L19-L27) you can see the parameters called number of AZs. This parameters you can pick the amount of avaliablity Zones you would want to deploy your application in which would later help with creating subnets. 

### 3.2 Mappings:
[Mappings](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html) are used for Key value stores. It can be used for storing certain values based on region which cannot be found through the documentation for example specific AMI IDs in specific regions. In For example in [Line 63-100](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L63-L100) this is something that can be observed and is used for getting the right AMI based on the region for the Launch Configuration.

### 3.3 Conditions:
[Conditions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/conditions-section-structure.html) can be used for conditional creation of certain resources. The conditions section in the code can be used for conditions are often used in the template and where in could save time and code to just create a reference to the condition instead. In For example in [ASG_And_ALB.yaml](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L109-L112) this is used to decide whether to build a piblic subnet or not. You might ask why do you not just less than ( < ) sign. Well CloudFormation does not have that so we have to evaluate using [Condition Intrinsic Functions]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html#intrinsic-function-reference-conditions-and](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-conditions.html#intrinsic-function-reference-conditions)). 


### 3.4 Resources:
In the [resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resources-section-structure.html) section you actually start to define which resouces you want in you cloudFormation Stack. The documentation explains it really I feel like I would do a disservice if I did not refer to that one.
//Write overall Structure
### 3.5 VPC, IGWs and Attach IGW
Once you understand the structure of a resource it is relativly simple creating an [VPC](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html) , [IGW](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-internetgateway.html) and [attaching the IGWs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc-gateway-attachment.html). In [Line 135-136](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L135-L136) you might notice something like [!Ref]([https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html)). Ref is like a intrinsic function and helps with refering to different different resources. In [Line 135-136](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L135-L136)  you can see it being used to refer to the VPC and the InternetGateway so that the cloudFormation understand which VPC and which IGW it should use.


### 3.6 Subnets
Subnets are used as a logical seperation between different resources in a VPC. It can also be used to attach certain resouces to different Avaliability Zones. In [Line 143-180](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L143-L180) you might notice that I name them as public subnets. This is because the user data for the EC2 instances require an internet connection. You might then ask why not use a NAT gateway? Well, that cost money which I do not have. So making them public was just to save cost. To be clear we have not made the subnets public currently we have just created them. The Subnets also have a condition. This is important since the condition section is used to determine whether to not to create the resource.  [!GetAZs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getavailabilityzones.html)  and [!Select](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-select.html) are intrinsic functions that help with chosing an avaliablity zone for each subnet. We also created a subnet for the ALB.

### 3.7 Route Table, Route and Route Table Association
[Route Table](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-route-table.html) are used to direct flow of traffic within a VPC. Route Tables require [Routes](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-route.html) to establish where they are supposed to direct traffic.  [Route Table Association](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-subnet-route-table-assoc.html) associate that specific Route to a Route Table.

In the template [Line 193-199](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L193-L199) we create a route out to the IGW so that instance and also attach the route to the Route Table.  In the template we can also see that for public Subnet B,C and D we only create an association if we have created that subnet. This can be observed in  for example [Line 210](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L210). 

### 3.8 Security Groups
[Security Groups](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html) are used as a firewall at the instance Level. In the Template we create two SGs, one to allow HHTTp traffic from the internet and one to allow Traffic from an SG. In the template [line 249-259](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L249-L259) refer to allowing all HTTP traffic this one will be used for our ALB.  The second SG is used for our instances that our autoscaling group will create which can be found [Line 261 - 272](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#L261-L272).

### 3.9 Target Group, ALB , ALBListener
In this section we will look at the creation of the [Target Group](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-targetgroup.html) , [ALB](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-loadbalancer.html) and [ALBListener](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-listener.html). The one thing I would like to mention is the fact that we assign a SG on [line 295](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#295) to the ALB.  Another thing to note is on [line 298-300](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#298-300) . In this part we only add a subnet if the subnet is created otherwise we return a [AWS::NoValue](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html#cfn-pseudo-param-novalue) which bascially return nothing/removes a resource property. This makes sure no issues occur when creating the template. 

### 3.10 Autoscaling Groups, Launch Configuration
In this section look at the creation of the [Launch Configuration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-as-launchconfig.html) and the [Autoscaling Group](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-as-group.html). The most imporant aspect to notice is the [user data](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#337-344). The user data basically an HTTP server with a webpage that has the host name in it. The user data has to be in [Base64](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-base64.html) which is why we use that intrinsic function. The other aspect is that we used [!FindInMap](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-findinmap.html), this intrinsic function helps us find a specific value in the mappings in out case it is the Region AMI name.

### 3.11 Outputs
Outputs is used for outputting specific metric that you want out of you template or you want to reuse in another template. In the template [Line 373-376](https://github.com/Internet-Person-IP/CloudFormation-ALB-ASG-Project/blob/master/ASG_And_ALB.yaml#373-376) we can see that I want the Application Load Balancers DNS since I want to try and see if I can access the webpage.


## 4. How To Use it?
You can use the AWS console and upload it there. Pick the parameters. Make sure to have a Role for your CloudFormation template. Click Create Stack. 

You can use the CLI aswell but I dont know how LUL.

In the output section of your CloudFormation stack you will be able to see the DNS name for the ALB and you should be able to use to check that you get a webpage.

[comment]: <> (## 5. Possible Improvements)