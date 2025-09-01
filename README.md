# Autoscaling
Project on autoscaling in AWS
Got it — you’d like the Markdown guide written in a **personalized style** using **“I”** instead of an impersonal step-by-step. Here’s the updated version:

---

````markdown
# How I Created Auto Scaling with Load Balancer and Target Group in AWS

This is my personal step-by-step process for setting up Auto Scaling in AWS, creating a Target Group, and attaching it to an Application Load Balancer (ALB).

---

## Prerequisites I Used
- An AWS account
- IAM user with administrative privileges
- A VPC with public and private subnets
- Security groups already set up
- An EC2 key pair for SSH access

---

## Step 1: Creating My Target Group

1. I went to **EC2 Dashboard** > **Target Groups**.
2. I clicked **Create target group**.
3. For the configuration:
   - Target type: **Instance**.
   - Name: `my-target-group`.
   - Protocol: **HTTP**, Port: **80**.
   - VPC: I selected my VPC.
4. For health checks, I kept it on HTTP and the path `/`.
5. I clicked **Next** and skipped registering instances since Auto Scaling would handle that.
6. I finished with **Create target group**.

---

## Step 2: Creating My Application Load Balancer (ALB)

1. I went to **Load Balancers** and chose **Application Load Balancer**.
2. I named it `my-alb` and made it **internet-facing**.
3. I added a listener for HTTP port 80.
4. I selected at least two subnets across different Availability Zones.
5. For security groups, I allowed HTTP traffic.
6. Under listeners and routing, I forwarded traffic to `my-target-group`.
7. Then, I created the ALB.

---

## Step 3: Creating My Launch Template

1. From **EC2** > **Launch Templates**, I chose **Create launch template**.
2. I named it `my-launch-template` and picked an AMI (Amazon Linux).
3. Instance type: **t2.micro** (small and cost-effective).
4. I attached my key pair for SSH access.
5. For security groups, I allowed inbound HTTP (80) and SSH (22).
6. I added this user data script so every instance runs a simple web server:

   ```bash
   #!/bin/bash
   yum update -y
   amazon-linux-extras install -y nginx1
   systemctl start nginx
   echo "Hello from my Auto Scaling instance!" > /usr/share/nginx/html/index.html
````

7. I created the launch template.

---

## Step 4: Creating My Auto Scaling Group

1. I opened **Auto Scaling Groups** and clicked **Create Auto Scaling group**.
2. I named it `my-asg` and selected my launch template.
3. I picked the same VPC and multiple subnets across AZs.
4. I attached it to the load balancer by choosing my **target group**.
5. For group size:

   * Desired capacity: 2
   * Minimum: 1
   * Maximum: 4
6. I set scaling policies (for example, scale out when CPU > 70%).
7. I clicked **Create Auto Scaling group**.

---

## Step 5: Verifying My Setup

* I checked **EC2 Instances** and saw new instances launched by Auto Scaling.
* In my **Target Group**, the instances showed up as healthy.
* I grabbed the ALB’s **DNS name** and opened it in my browser.
* I saw:

  ```
  Hello from my Auto Scaling instance!
  ```

---

## Step 6: Testing Auto Scaling

* I applied load to the instances.
* I watched Auto Scaling automatically spin up new ones.
* When the load dropped, AWS scaled them back down.

---

## My Summary

By following these steps, I was able to:

* Create a **Target Group**
* Set up an **Application Load Balancer**
* Build a **Launch Template**
* Deploy an **Auto Scaling Group**

Now my application is fault-tolerant, highly available, and scales automatically when traffic changes.

```

---

Do you want me to also **draw the architecture with a Mermaid diagram** so the Markdown shows a nice flow of how ALB, Target Group, and Auto Scaling connect?
```
