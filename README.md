# Deploying Active Directory in Azure

<p align="center">
<img src="https://github.com/user-attachments/assets/0bb09e28-c161-451f-a72d-9e0315fc433d" alt="Microsoft Active Directory Logo"/>
</p>


<h1>Deploying Active Directory in Azure</h1>
In this lab we will be installing Active Directory on the Infrastructure we set up in the previous steps section

## Previous Steps

[Preparing Infrastructure for Active Directory in Azure](https://github.com/jeremiahgriffit/Infrastructure-For-AD-Azure)

<h2>Operating Systems Used </h2>

- Windows Server 2022 Datacenter: Azure Edition - x64 Gen2  
- Windows 10 Pro, version 22H2 - x64 Gen2  

<h2>High-Level Deployment and Configuration Steps</h2>

1. Install Active Directory
2. Promote Domain-Controller-1 to the domain controller
3. Create Users and Admins Organizational Units
4. Configure Jane Doe as a Domain Admin
5. Join Client-1 to the domain
6. Verify Client-1 Joined the domain


# Deploying Active Directory in Azure

### Install Active Directory 
#### Connect to Domain-Controller-1
Virtual Machines > Domain-Controller-1 > Networking > Network settings 

From here we want to take note of the public IP address we will be using this to remote into Domain-Controller-1

![18](https://github.com/user-attachments/assets/e7e60b6e-0fe5-4f7f-9fb2-396aa86e7c11)

Using Remote Desktop Connection enter the public IP you found in the last step  
Username: LabADMIN  
Password: LabPassword123  
Then click "connect"

![19](https://github.com/user-attachments/assets/05c136e9-4118-496f-a9ca-247459e5c0fb)

- When you connect you should be met with this screen  
- To begin installing Active Directory click on "add roles and files"

![1](https://github.com/user-attachments/assets/9bc4aa84-2eb3-46e7-bfe3-90d73462f764)



- Next we will be met with the activation wizard we can click next on all prompt until we get to the "Server roles" secction

![2](https://github.com/user-attachments/assets/175f599c-9744-464e-99cd-c4a74e39680e)


 - On the "Server roles" section:
    -  check the highlighted "Active Directory Domain Services" box
    -  accept any addtional prompts and hit next until you get to "Confirmation"

![3](https://github.com/user-attachments/assets/cf2b7b6e-d2df-437a-8922-f5d9bc9de4f3)

 - Check the "Restart the destination server automatically if required" and click install   

![4](https://github.com/user-attachments/assets/dd1de4dc-5a74-4426-a7da-f6dcf125cd07)


### Promote Domain-Controller-1 to the domain controller 

- Now we must promote this VM to domain controller to do that
   - Select the flag icon on the top right of the screen and click on "Promote this server to a domain controller"

![5-cropped-resized](https://github.com/user-attachments/assets/4fa706eb-2e86-4b74-948a-f34d6929b93f)


- Enter "exampledomain.com" into the root domain name

![6](https://github.com/user-attachments/assets/26e3a4b5-68b4-4a58-8f26-f5683cf1b29d)


- Next choose a password for the Dirrectory Services Restore Mode
   - I used: LabPassword123 to keep lab passwords consistent


![7](https://github.com/user-attachments/assets/13194115-8d5b-4bd9-af07-805f5ea3d4d9)


- uncheck "Create DNS delegation" when met with this prompt

![8](https://github.com/user-attachments/assets/69c9cd7f-9d71-4781-8d05-44eb5d861984)


- Click next until you get to Prerequisites Check and click install
   - After installtion the VM will restart and you will loose the RDP connection momentarily     

![9](https://github.com/user-attachments/assets/041594fa-375f-464c-bfea-f12c3104497c)


### Create Users and Admins Organizational Units 

Navigate to "Active Directory Users and Computers"  
![10](https://github.com/user-attachments/assets/657fa35d-c384-453f-be04-7172b9d812f3)


Create a new Organizational Unit (OU)

![11-cropped-resized](https://github.com/user-attachments/assets/cdccb169-88e0-4ff2-82bb-b46acfc09ee4)

Name The OU _EMPLOYEES 

![image](https://github.com/user-attachments/assets/4f856e40-5a26-437d-b8fd-b5ba9b923927)

Create a new OU called _ADMINS

![image](https://github.com/user-attachments/assets/3a4cc40c-2c7a-40bb-8e3e-7085446889b0)

Create a new user in the admin OU

![12-cropped-resized](https://github.com/user-attachments/assets/f5c64614-7a2d-42ca-9c22-67089b871e6b)

Name the new User jane doe and confiure the userlogon as jane_admin@exampledomain.com then click next

![image](https://github.com/user-attachments/assets/a1f377cf-4beb-4f7e-bb6c-a560ffbba33c)

For the password I used LabPassword123 
untick the "User must password at next logon" option to save some time for lab purposes 
Tick "Password never expires" (for lab purposes)
Click next then finish on the following prompt

![image](https://github.com/user-attachments/assets/66f878d0-0c62-4b60-a71b-908f94215da6)


### Configure Jane Doe as a Domain Admin 

Just because we added jane doe to the _ADMIN OU does not mean that jane doe has admin privlages  
In order to promote jane to admin we must add her user to the "Domain Admins" Security group  


Right click jane doe and click "Properties"

![13](https://github.com/user-attachments/assets/34edda55-57a1-4f9c-a1f1-e3ae4bc739ab)


Click "member of" tab then click "add"

![image](https://github.com/user-attachments/assets/f24190a3-f3ea-4b23-9abb-7bd95f80bfbd)

Type "Domain Admin" then click "Check Names" and "OK" to finalize 

![image](https://github.com/user-attachments/assets/ca5827b8-ea38-4e8a-9f44-ba2ed52216dd)

Next we will log out and resign in as the jane_admin user

To do this press windows key + R and type "logoff" to logoff

![image](https://github.com/user-attachments/assets/03619b64-02ae-4318-8e02-32b99e2644fe)

### Domain Context

When logining in as jane_admin it is useful to understand why we are loging in using "EXAMPLEDOMAIN\jane_admin"

The reason is because jane_admin exists in the EXAMPLEDOMAIN domain for this reason we add the domain context to the login

Hypothecially if jane_admin were to exists in companydomain.com we would then use the domain context "companydomain" instead

![AD-DOMAIN-CONTEXT](https://github.com/user-attachments/assets/33f4ef12-0c6b-4f62-acfb-ba92e9ca8ef8)

Login as jane_admin  
username: EXAMPLEDOMAIN\jane_admin  
Password: LabPassword123  
Click "connect"  

![image](https://github.com/user-attachments/assets/e38c5bbb-c72f-4d95-ad1d-86949f38953c)

### Join Client-1 to the domain
Connect to Client-1 

Find client-1 public IP address:  
Virtual Machines > Client-1 > Networking > Network settings 

![image](https://github.com/user-attachments/assets/35965722-d05f-4c90-be6c-a737d71cd161)


login using Remote Desktop Connection

![image](https://github.com/user-attachments/assets/9466d64c-5c51-404f-afd6-e4684a9c70ab)

Right click windows icon > system

![14](https://github.com/user-attachments/assets/0ef2c390-13b8-459f-af70-904f2809a33e)

Click "rename this PC (advanced)

![15](https://github.com/user-attachments/assets/e5167cb3-4264-40ba-a515-b9c23d2f967c)



Then click "Change.."

![image](https://github.com/user-attachments/assets/a4c5ac1d-acb1-4130-b3e3-a06eadb1c609)

Select Domain and type "exampledomain.com to join the domain

![image](https://github.com/user-attachments/assets/796d9f9f-eb7c-493e-8aba-8de25ee15e91)

Now we can just jane_admin to authorize the joining of the domain
Username: EXAMPLEDOMAIN\jane_admin
Password: LabPassword123

![image](https://github.com/user-attachments/assets/18204e03-1ea8-4ee2-9e74-58e315c41ea4)

If done Correctly you should be met with this screen, as well a restart notification.  
Once the VM restarts it will be in the EXAMPLEDOMAIN.COM domain

![image](https://github.com/user-attachments/assets/6d7afd4c-6e8c-4264-a6f2-80d3f24f5ea0)
![image](https://github.com/user-attachments/assets/5091c340-c9d6-4e28-9504-2583865d45df)

### Verify Client-1 Joined the domain
Navigating back to Domain-Controller-1 > Active Directory Users and Computer  
We can see that Client-1 Joined the domain  
Futhermore we can see details on client-1 such as the Operating System it currently has installed

![image](https://github.com/user-attachments/assets/e6f6689a-2699-4469-bddc-3e908ae6488f)



