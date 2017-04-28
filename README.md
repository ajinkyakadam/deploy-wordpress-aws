## Deploying Wordpress Site using Ansible and CloudFormation

We deploy a wordpress site using Ansible and CloudFormation Template. You should have the following Pre-Requisites installed on the ansible machine. The total time to setup would be around 20~25 minutes. 

Pre-Requisites:
- git
- python-boto
- ansible

We buid the following stack 
  - 1 VPC 
      - Creates a default "Main Route Table", "Security Group", "Network ACL"
  - 1 Public Subnet inside VPC 
  - 2 Private Subnets inside VPC  
  - 2 Security Groups (One for WebServer, One for DB Instance)
      - DBSecGroup : Allows traffic only on DB port 
      - WebServerSecGroup : Allows traffic on SSH (22) and HTTP(80) ports   
  - 1 EC2 Instance for WebServer ( Public Subnet )  
  - 1 RDS Instance for Wordpress Database ( Private Subnet ) 
  - 1 Interet Gateway 
  - 1 Route Table and 1 Route for the internet attached to the Public Subnet

<img src="Wordpress Deploy.png">


### Instructions to Use (For Linux-Ubuntu Users)

Lets first create a keypair by logging into our amazon console. Remember the name of the keypair. We will use the created keypair to ssh into ec2 instances. 

Next lets setup the credentails which we will use to provision resources. You need the `access_key` and `secret_access_key` 
Create a folder name `.aws` in your home directory, using the followiing command

```
mkdir ~/.aws/
```

Next create `credentails` file in `~/.aws/` folder, using 

```
touch credentails
```

Now lets add our keys to the credentails file. Replace `your_access_key` and `your_secret_access_key` with the keys provided to you. Add these lines in your credentails file.

```
[default]
aws_access_key_id=your_access_key
aws_secret_access_key=your_secret_access_key
```

Now lets download this repository. Run 

```
git clone https://github.com/ajinkyakadam/deploy-wordpress-aws.git
```

Add the following to file `/etc/ansible/hosts` 

```
[local]
localhost
```

Then, 

```
cd deploy-wordpress/
```

and provision the stack run the following,

```
ansible-playbook -i /etc/ansible/hosts deploy-wp.yml --verbose
```

Once the stack creation is complete open your browser and goto  (replace WebServerPublicIp with your WebServers IP)

```
http://WebServerPublicIp/wordpress/wp-admin/install.php
```

You will see the setup page as below 

<img src="startup.png">

Next, after you select the language of your choice, enter your details 

<img src="Details.png">

Now you are done. You will see the following,

<img src="SetupComplete.png">

### References
