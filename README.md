# amazonaws
Instructions on how to use the AMI using amazonaws

Here are instructions on setting up and using the OpenMBEE AMI on AWS.

1. Sign up for an [Amazon AWS account](https://aws.amazon.com/).
* Sign in to your account.
* Click on **EC2 Virtual Servers in the Cloud**.
* Click on **Launch Instance**.
* In **Step 1: Choose AMI**:
 * Click on **Community AMIs**.
 * Search for **openmbee**.
 * Click on **Select** next to the openmbee AMI.
* In **Step 2: Choose an Instance Type**:
 * Click on **m3.xlarge**. This is the smallest recommended instance for decent performance.
   * This is roughly $200 a month. See [pricing calculator](http://calculator.s3.amazonaws.com/index.html) for more details.
 * Click on **Next: Configure Instance Details**.
* In **Step 3: Configure Instance Details**, click on **Next: Add Storage**.
* In **Step 4: Add Storage**, click on **Next: Tag Instance**.
 * 40 GB is the default for the AMI, you may need to update this later (instructions to be added...).
 * Tags are useful for tracking and naming your instances if you have more than one.
* In **Step 5: Tag Instance**, click on **Next: Configure Security Group**.
* In **Step 6: Configure Security Group**:
 * Create a new security group, name it however you like. Add the following inbound rules:
  * Custom TCP Rule / TCP:
   * Ports: 22, 80, 443, 8443
    * These are for SSH and Tomcat server
     * If you want to restrict access for particular subdomains, specify it in **Source**
 * Click on **Review and Launch**.
* In **Boot from General Purpose (SSD)**, click on **Next**.
* In **Step 7: Review Instance Launch**, click on **Launch**.
* In **Select an existing key pair or create a new key pair**:
 * In first drop down, select *Create a new key pair* (if you've already created a key pair, choose that one).
 * Enter a key pair name, then select **Download Key Pair** (it will be a **.pem** file, we'll call it **mykey.pem** from now on).  
 * **MAKE SURE YOU KEEP THIS IN A SAFE PLACE AS THIS IS WHAT YOU WILL USE TO LOGIN AS AN ADMINISTRATOR ON YOUR INSTANCE.**
 * Click on **Launch Instances**.

Your instance will take some time to come up, you can monitor it in the EC2 console. Once it's up, it's time to SSH into it and start it up.

1. Make sure the permissions on **mykey.pem** are 400 (e.g., only readable by you, otherwise SSH will ignore it).
 * chmod 400 mykey.pem
* SSH into your machine (under your instances dashboard you should be able to find your IP address)
 * ssh -l ec2-user -i mykey.pem aws_ip_address
  * Permanently add the pem to your ssh keys, by
   * moving mykey.pem to ~/.ssh
   * ssh-add mykey.pem
* ALMOST THERE
 * follow the README in /opt/ on how to configure
 * mostly if things aren't already running, you need to start postgresql and tomcat (run following as root)
   * /etc/init.d/postrgresql start
   * /etc/init.d/tomcat start
       
