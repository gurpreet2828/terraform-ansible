### Summary: 

This Project basically focusing on managing AWS infrastructure using Terraform (Infrastructure as Code, IaC) and Ansible (Configuration Management, CM).

The steps include generating SSH keys, installing necessary tools like PuTTY and PuTTYgen, and setting up the Terraform structure and code for provisioning EC2 instances. Ansible is used for automation, such as installing Apache on the EC2 instances

This guide outlines the steps to set up Terraform, AWS CLI, and deploy an EC2 instance on AWS using Ubuntu. The process starts with checking your Ubuntu version and installing Terraform and the AWS CLI. After configuring the AWS CLI with your IAM credentials, transfer your Terraform and Ansible files to the Ubuntu machine using scp and adjust permissions. You also install Visual Studio Code and the Terraform extension for ease of configuration. After that, use Terraform commands (init, validate, fmt, plan, and apply) to deploy an EC2 instance. Troubleshooting tips include addressing common issues such as AWS CLI configuration errors, permission denials during SSH, and security group settings for SSH access.

The project goal is to demonstrate the integration of Terraform and Ansible for efficient cloud infrastructure management. The final report includes screenshots and outputs showing the successful setup.

### Troubleshooting:

1.	**AWS CLI Configuration Problems:** If aws configure fails, verify that your AWS Access Key and Secret Key are correct.

2.	**Permission Denied During SSH:** Ensure you're using the correct private key and the correct username (ec2-user, ubuntu). Also, ensure the security group allows SSH access. And ensure you're using the correct private key with the -i option.

3.	**Terraform Plan Errors:** Check if the paths to private/public keys are correctly defined in the variables.tf file and confirm that AWS credentials have sufficient permissions.

4.	**Security Group Issues:** Ensure the EC2 instance's security group allows inbound SSH traffic on port 22 from your IP address.

5.	**Incorrect SSH Key Permissions:** Run chmod 600 /path/to/your-private-key.pem to fix key permission issues.

6.	**Access Denied to Files:** Ensure the directory has the correct permissions by running sudo chmod +rwx /home/terraform/Ansible-Terraform/.



**1. Check Ubuntu Version:**

Run the following command to check your Ubuntu version:

Run – cat /etc/os-release to check ubuntu version

![Picture18](https://github.com/gurpreet2828/terraform-ansible/blob/7484934dcf9a99bdeab89b7e0bf4dd0633f1d990/Images/Picture18.png)

### Step 1: Installation of terraform

**Install Terraform (Linux -Ubuntu)**

**Run the following commands**

wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update && sudo apt install terraform

**terraform –version**

You will see the following screen and verify the terraform installation

 ![Picture1](https://github.com/gurpreet2828/terraform-ansible/blob/7484934dcf9a99bdeab89b7e0bf4dd0633f1d990/Images/Picture1.png)


**On Linux – Install AWS Cli**

To install the AWS CLI, run the following command

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install

Run the following command to check if AWS CLI is installed correctly:

aws –version

You see the following output
 
![Picture2](https://github.com/gurpreet2828/terraform-ansible/blob/e865a39b270aed465e57309ff0554314a056a5b2/images/Picture2.png)

### Step3: Create AWS account 

After Creating 

Click on account name - Select Security Credentials
 
![Picture20](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture20.png)

Click Create access key.

![Picture21](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture21.png)
 
Note: Download the key file or copy the Access Key ID & Secret Access Key (Secret Key is shown only once!).

### Step 4: Configure AWS CLI with the New Access Key

aws configure

It will prompt you for:

**1.	AWS Access Key ID:** Your access key from AWS IAM.

**2.	AWS Secret Access Key:** Your secret key from AWS IAM.

**3.	Default region name:** (e.g., us-east-1, us-west-2).

**4.	Default output format:** (json, table, text — default is json).

As shown bellow
 
![Picture3](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture3.png)

**Check credentials added to aws configure correctly**

aws sts get-caller-identity

If your AWS CLI is properly configured, you'll see a response like this:
 
![Picture4](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture4.png)

**Transfer Terraform and Ansible Code:**

Use scp to transfer your Terraform and Ansible files from your local machine to your Ubuntu instance:

Note: Transfer the terraform and ansible code from local machine to Linux by running following command on cmd 

scp -r "C:\Users\Gurpreet\OneDrive\Desktop\York Univ\Assignments\Assignment 3- Manage AWS Infrastructure with Terraform (IaC) and Ansible (CM)\Ansible and Terraform" terraform@10.0.0.251:/home/terraform

Enter your password of ubuntu user

![Picture5](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture5.png)
 
It shows the following screen after transfer

![Picture6](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture6.png)
 
You will see the following ‘Ansible and Terraform’ folder created in your Ubuntu Linux Machine

![Picture7](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture7.png)

**Change permissions:**

Created another folder name ‘Ansible-Terraform’ and give the permission by running the following command

sudo chmod +rwx /home/terraform/Ansible-Terraform/

**Step 5: Install vs code**

**Step 6: Install Terraform Extension in VS Code**

1.	Open VS Code.

2.	Go to Extensions (Ctrl+Shift+X).

3.	Search for "HashiCorp Terraform" and click Install.

4.	Once installed, restart VS Code.

**Step 7: SSH**

I am using remote explorer on vs-code for accessing the Ubuntu- linux machine remotely 
 
![Picture9](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture9.png)
 
![Picture10](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture10.png)

### Generate SSH Keys

This will be used later to connect to EC2 instances in your code and using ssh terminal

Open a terminal window. At the shell prompt, type the following command:

**ssh-keygen -t rsa**

The ssh-keygen program will prompt you for the location of the key file. Press Return to accept the defaults.

Press enters twice for no passphrase

Note the location of your public and private keys were saved, they will be required in a subsequent step


### Deploy EC2 instance on AWS through Terraform

Run the following commands

**1.	Terraform init**

•	terraform init is a command used in Terraform to initialize a working directory containing Terraform configuration files

•	It sets up your environment by configuring the backend, downloading necessary provider plugins, and installing modules, 
allowing you to then plan, apply, and manage your infrastructure

You will see the following screen after running Terraform init
 
![Picture11](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture11.png)

**2.	Terraform validate**

•	The terraform validate command checks whether your Terraform configuration is syntactically valid and internally consistent

•	Ensures your .tf files are written correctly (Syntax Checking) (valid HCL syntax)

•	Verifies references between resources are valid.

•	Does not check whether your AWS credentials or resources exist — that happens during plan and apply.

 ![Picture12](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture12.png)

**3.	Terraform fmt**

•	The terraform fmt command automatically formats your Terraform code to follow canonical style conventions.

•	Fixes indentation

•	Orders attributes consistently

•	Makes your .tf files cleaner and easier to read

•	Applies to files in the current directory by default



**4.	Terraform plan**

terraform plan is a crucial command in Terraform that allows you to preview the changes that Terraform intends to make to your infrastructure based on your current configuration and state

**Generates an Execution Plan:** Terraform produces a detailed plan outlining the proposed actions. This plan shows: 

Which resources will be created (+ symbol).

Which resources will be modified (~ symbol, showing the before and after values of changed attributes).

Which resources will be replaced (-/+ symbol, indicating destruction and creation).

Which resources will be destroyed (- symbol).

If it shows the following error 

![Picture13](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture13.png)

If you encounter issues, ensure the paths to private/public keys are correctly specified in your variables.tf file.
 
Then you put path of the private and public keys to the variables.tf in compute folder

As Shown bellow

![Picture14](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture14.png)

Then execute the terraform plan

**5.	Terraform apply**

•	terraform apply is the command in Terraform that executes the actions proposed in the execution plan to create, modify, or destroy infrastructure resources. It's the command that actually makes changes to your real-world infrastructure
 

Now you can access the website from browser using the URL provided in the output 

![Picture16](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture16.png)

Verifying ec2 instance running on AWS

•	Login to aws Account

•	Go to EC2 – Instances
 
![Picture17](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture17.png)

Accessing or connect to ec2 instance 

ssh -i /root/.ssh/ansible ec2-user@44.203.32.76

you will see following screen

![Picture19](https://github.com/gurpreet2828/terraform-ansible/blob/c9a4187e37f3966c1b4e19068e6f991ec523cf61/images/Picture19.png)
 

Ansible and httpd server automatically installed in ec2-instance















