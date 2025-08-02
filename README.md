# Manual Deployment of Java Web App on EC2 with ALB, ASG and S3 Artifact

This project demonstrates a **manual deployment** of a Java-based web application (`vprofile`) to an EC2 instance using `user-data`.  
The artifact was built locally, uploaded to S3, and deployed via a script at instance launch.  
The setup includes an **Application Load Balancer**, **Auto Scaling Group**, proper IAM permissions, and a **private hosted zone** in Route 53.

---

##  Stack Used

- **Amazon EC2** (Amazon Linux 2023)
- **Amazon S3** (artifact storage)
- **Application Load Balancer (ALB)**
- **Auto Scaling Group (ASG)**
- **Security Groups**
- **Route 53 (Private Hosted Zone)**
- **MariaDB**, **RabbitMQ**, **Memcached**
- **Apache Tomcat 10**
- **Maven** (for building the `.war` file)

---

##  Deployment Flow

1. Application code cloned and built locally using Maven:

   ```bash
   mvn clean install
The generated .war file was uploaded to an S3 bucket:


aws s3 cp ./target/vprofile-v2.war s3://vprofile-las-artifacts0010/
A user-data script was used to:

Install Tomcat 10

Download the .war file from S3

Deploy it to /var/lib/tomcat10/webapps/ROOT/

Start the Tomcat service

The EC2 instance was registered to a Target Group behind an ALB.

An AMI was created from the configured EC2 instance.

An Auto Scaling Group was launched using a Launch Template from that AMI.

 Security
All services use isolated Security Groups with only necessary ports open:

HTTP (80)

SSH (22)

MySQL (3306)

Memcached (11211)

RabbitMQ (5672)

 DNS and Access
A Route 53 private hosted zone was created for internal resolution.

The app is accessible internally via:
http://app01.vprofile.in

Screenshots
✅ App Running on ALB
✅ RabbitMQ + User Info
✅ Auto Scaling Group + Security Groups

✅ EC2 Instances + ALB + AMI

✅ Security Group Rules
✅ EC2 Manual Deployment & Artifact from S3

✅ Tomcat + WAR Installation

Notes
This deployment is entirely manual and does not use CI/CD pipelines.
Ideal as a learning project for:

EC2 provisioning

ALB + ASG integration

Basic automation via User Data

For future iterations, this setup can be extended using CodeDeploy, CodePipeline, or Terraform.

 Author
Roberto Rodríguez
AWS Certified Solutions Architect & DevOps Engineer
github.com/kuota1

