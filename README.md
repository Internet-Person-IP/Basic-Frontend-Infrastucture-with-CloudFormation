# CloudFormation Basics Project
![Image]([https://xebialabs.com/wp-content/uploads/files/tool-chest/cloudformation.jpg](https://xebialabs.com/wp-content/uploads/files/tool-chest/cloudformation.jpg))
## 1. Introduction
This project was built as a way for me to learn cloudformation and to apply certain concepts learned in AWS in cloudformation. The cloudformation stack creates a VPC, an ALB, an ASG, a small HTTP server and creates all the neccasary routes and SGs for the application to work. 

## 2. Purpose
The purpose was to learn about how to create a cloudformation stack and also be able to apply them to create a basic template that I could reuse if ever needed. This basic template can be explanded upon and can be used to create larger projects. Hopefully this can also be used to help people learn abit about how cloudformation works.

## 3. Project and Concepts Applied 
In this section I will be going over the template from Top to Bottom and giving Links of documentation that should help you with understanding the different concepts. It should help you learn how to the documentation can be applied in a projects. **This Project is a barebones project and does NOT go over all documentation but the purpose is more to see how it can be applied to create a CloudFormation template.**

## 4. How To Use it?
You can use the AWS console and upload it there. Pick the parameters. Make sure to have a Role for your CloudFormation template. Click Create Stack. 

You can use the CLI aswell but I dont know how LUL.

In the output section of your CloudFormation stack you will be able to see the DNS name for the ALB and you should be able to use to check that you get a webpage.
## 5. Possible Improvements
