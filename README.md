# Host-Website-and-Monitor-using-Cloudwatch

![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/8c886e4f-7577-4773-9ded-db1f4147ef0a)


**We will host Apache2 on an EC2 instance and monitor it using CloudWatch. We will receive an alert from SNS whenever the CPU utilization goes above 80%.**

**Step 1 : Create an EC2 instance.**

- Launch an instance using AMI-UBUNTU, t2.mirco, default network settings and user data as follows

![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/5859c45b-0d40-4f8e-a494-2963d6832ad5)

User data : This user data includes installation of apache2 and creation of index.html file

```bash
#!/bin/bash
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
sudo rm /var/www/html/index.html
sudo touch /var/www/html/index.html
sudo chmod 777 /var/www/html/index.html
echo "<header> <h1>Welcome to my webpage!</h1> </header>" > /var/www/html/index.html
sudo systemctl restart apache2

```


Without logging into the instance, I can browse m application thats the power of userdata (automation)

![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/42970dd9-f9de-4fc0-a1ba-ac127ba51044)



**2. Create SNS**

  - Create SNS Topic.
     * Navigate to the SNS dashboard and create a topic.Choose a name based on usecase, here we are using for for CPU alert notification. So, I am naming it as CPU-    
       Alert. 
    
       ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/f9d01e3c-63a5-449f-bb7e-0dda724879dc)
       
     * Select Standard in Typeto get nofified by email.
       
       ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/50f15ea5-5e80-4e2b-88e1-45a26e24e62e)
  
       Once the SNS topic is created you can add the subscribers  to the topic and they will be notified by the email when the alarm gets trigerred.
       
       ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/25e3f259-f048-4d21-a2bc-0fd016893e32)
     
  - Create Subscription.
     
       * Select email in protocol section, since we want to get notified by email.
       * Enter the email address in the Endpoint Section.
         
         ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/0d2bb762-0252-4172-9bfc-05149fe7a4d0)
  
      * Once you create the subscription you can see the page as below and status would be pending confirmation.
  
         ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/71af5032-c921-4c94-93f3-d8a144f1f618)
        
      * Confirm the subscription  through the link sent to the email address.
        ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/4caa51c6-4698-4e4a-8db5-9ec639284ab9)
  
        ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/c3ce297e-f29d-4b62-8a4a-cdb98c6738d1)
  
      * Now you can see the status as confirmed.
        ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/542bd5d9-5dde-4fc3-b521-01a0cf30e160)

**Step 3: Configure CloudWatch.**

  - Go to the EC2 Dashboard, select your instance -> Actions -> Drop down and select -> Monitor and troubleshoot -> Manage CloudWatch alarms.
    
    ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/a5737a73-5187-4c4e-b18a-4422866b36e8)

 - Add CloudWatch Alarm -> Create Alarm and select the SNS topic we have created.
   
   ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/9467caff-679a-4055-9249-7d7301d41b88)

   Now let's configure the CloudWatch Alert if the Max CPU utilization is more than or equal to 80%.

   ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/957f77b4-3cd6-47b2-99a4-9c0e74dc1b76)

* When CPU utilization is at least 80%, the CloudWatch alarm triggers and we receive an email.
* 
<img width="683" alt="image" src="https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/590b8420-1563-45f0-b186-685d14d493ae">

* This is the clear detailed view of the CloudWatch alarm for the CPU utilization.

  ![image](https://github.com/Vicky2KR/Host-Website-and-Monitor-using-Cloudwatch/assets/115537512/68390a5a-fed9-4d7f-98e2-a7ccff4839bb)


!!!!! This is how you can monitor your website using CloudWatch !!!!!

*********************************************************************************************************************************************************************

Why is my server reaching more than 90% CPU utilization?

The answer is simple: stress.

You can use the stress command to test whether you are receiving an email alert when the CPU utilization goes above 80%.

To do this:

1. Install the stress package: sudo apt install stress -y
2. Run the stress command: sudo stress --cpu 2 -v --timeout 30s
* This will put a heavy load on the CPU for 30 seconds.

3. Check your email for an alert notification from SNS


  
