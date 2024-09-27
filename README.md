# 2420 Assignment 1
###### Student Name: Jovica Kuzmanovic
###### Student ID: A01339297
#
# Setting Up an Arch Linux Server on Digital Ocean
- This guide will walk you through the process of setting up a server on Digital Ocean using Arch Linux. We'll cover everything from creating an SSH key pair to configuring your server with cloud-init.
- The guide assume that you already have digital ocean account and that you posses the basic computer knoweldge,for example opening a terminal on your operating sytem(OS) and creating a directory on your OS.
#
## Requierments
- Digital Ocean Account
- A personal computer with an operating system
- Access to the Internet
- Arch linux .qcow image (Download link may be found here: [Arch Linux Image](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/1545). Please downloand the **.qcow** which size is approximatley 500MB.
#
## Topics:

- SSH: Secure Shell Protocol.
- Digital Ocean
- Cloud-init

## Table of Content:

1. Creating an SSH Key Pair.
2. Create a Project in your Digital Ocean account.
3. Creating a Cloud-Init Configuration File. 
4. Creating a Droplet.
5. Conneting via ssh to our droplet.

#
### Step 1: Creating an SSH key pair

## **Before we create our first Secure Shell Protocol(SSH) key, we need to learn more about SSH**
#### What is Secure Shell Protocol(SSH)?
- SSH is a method for securely sending commands to a computer over an unsecured network. The SSH uses cryptography to authenticate and encrypt connections between devices.
- SSH is secure because it incorporated encryption and authentication via process called public key cryptography. Public key cryptography is the way to encrypt the data, or sign data, with two different keys, one key is public, the other one is private kept by the owner.

#### What does SSH do? 
- SSH can do remote encrypted connections. SSH sets up a connection between a users device and a faraway machine, often a server. It uses encryption to scramble the data that traverses the connection.
- SSH also has abillty to tunnel. Tunneling is a method for moving packets across a network using a protocol or path they would not ordinarily be able to use. Tunneling works by wrapping data packets with additional information called headers to change their destination.

#### When do we use SSH?
- SSH is often used for controlling servers remotely, for managing infrastructure, and for transferring files.
#

#
#### **Create an SSH key pair**

1. Open the Terminal on your machine.
2. If you are using Windows as your OS you may need to create .ssh directory in your home directory first.
3. (Tilda)~ in on your terminal means that you are in your home directory.
    
Create a SSH key pair on **Linux** or **MacOS** Type the following command:
        
    ssh.keygen -t ed25519 -f ~/.ssh/do-key -C "youremail@address"
         
On **Windows** machine Type the following command:
    
    sh-keygen -t ed25519 -f C:\Users\your-user-name\.ssh\do-key -C "youremail@email.com"

         
The command above will create a two text files in your .ssh directory

**"do-key"** or **"do2-key"**    This is your private key
**"do-key.pub"** or **"do2-key"**  This is the public key, this key will be copied to a server.
#      
#### **Copying your Public Key**

Our keys are just a plain text files. In order to make sense of those files we will use our terminal.
Copying our public key is a straight forward process, depending on your OS, please **COPY** the command into your terminal.

For **Windows** users:


    Get-Content C:\Users\your-user-name\.ssh\do-key.pub | Set-Clipboard

      
For **MacOS** users:

    
    pbcopy < ~/.ssh/do-key.pub
 
For **Linux** users it will depend on the type of your distribution. Please refer to the documentation of your distribution. Here are some of the popular commands. Note that the first command requires [**Wayland**](https://wiki.archlinux.org/title/Wayland) instalation:
    
    wl-copy < ~/.ssh/do-key.pub

    xclip -selection clipboard < ~/.ssh/do-key.pub

#  
#### **Adding your public key to Digital Ocean**

1. Click on Settings located on the left side at your home page 
2. Click **Security** tab
3. Click **Add SSH Key** button which is located on the right side of the page 
![Security and ADD SSH](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/Settings.png)
4. In the **SSH Key Content** box, paste your **Public SSH Key**, give it a name of your choice. Good practice is to name your keys as something that you will remeber, for example **"[MyProject]"** of course use the name of your project.
![SSH](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/Image%203.png)
5. If everything went well you should have result similar to the image below.
![SSH creation](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/Image%204.png)
6. If you are facing any issues, you may start the process all over again starting from the first step, that should fix the problem.

> **Please note that before continuing to the next steps of this guide, you have to ensure that you complete the previous steps.**
#
### **Step 2:Create a Project in your Digital Ocean account**

 1. Click **New Project** in the menu located on the left side of your screen.
 2. Type **Name** of your project
 3. Select **Purpose** of your project
 4. Click **Create Project**

 ![New Project Image](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/new%20project.png)

Your Project should apper in the top left side menu, under the dropdown menu **PROJECTS**

 ![Projects](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/new%20project1.png)
#
### **Cloud-init configuration file**
#
## **Before we initialize a cloud-init file, we should learn more about cloud-init**

#### What is cloud init?
Cloud-init allows us to setup a server with some initial configuration. In order to configure cloud-init we shoudl create a configuration file.
One of the better practices is to create a YAML file.

#### What is YAML file?
[YAML](https://yaml.org/) is a human-readable data serialization language often used for configuration files, data storage, and inter-process communication. It's designed to be more readable than JSON and XML.

#### What does cloud-init do?
Cloud-init can handle a range of tasks that normally happen when a new instance is created. It’s responsible for activities like setting up the host name, configuring network interfaces, creating user accounts, and even running scripts. This streamlines the deployment process, your cloud instances will all be automatically configured in the same way, which reduces the chance to introduce human error.

#### How does cloud-init work?
The operation of cloud-init broadly takes place in two separate phases during the boot process. The first phase is during the early (local) boot stage, before networking has been enabled. The second phase is during the late boot stages, after cloud-init has applied the networking configuration.

Now when we understand what is cloud-init and what dows it do, we can continue to our next step
#

### **Step 3:Create configuration file cloud-init**

1. Run the commnad below in order ot confirm that clound-init is running:

       systemctl status cloud-init
2. Create cloud-init configuration in your .ssh folder. Yaml file needs to have an **.yml** extension
    **example: cloud-init.yml**
3. Edit cloud-init file in the text editor of your choice. You may use Notepad, Visual Studio Code, or any other program in which you can edit text.
4. Copy the example below and paste into your cloud-init configuration file, and make changes as instructed, save yaml file on your machine. 
    
        #cloud-config
        # This cloud-init configuration file defines various settings for your droplet.

        # Defines a new user with the specified username, group, shell, and sudo privileges.
        users: 
        -name: user-name # Change this to your desired username
        -primary_group: group-name # Change this to your desired group name
        -groups: wheel # Adds the user to the wheel group for sudo privileges
        -shell: /bin/bash # Sets the user's default shell
        -sudo: ['ALL=(ALL) NOPASSWD:ALL'] # Grants the user sudo privileges without requiring a password
        -ssh-authorized-keys: #paste here you public ssh key
            - ssh-ed25519 ...

        packages:
         # Specifies a list of packages to be installed on the droplet.
        - ripgrep
        - rsync
        - neovim
        - fd
        - less
        - man-db
        - bash-completion
        - tmux

        disable_root: true
         # Disables the root user, making it safer to manage the droplet.
#### Troubleshooting a cloud config file

If the configuration in your file is not being applied you probably won't see an error message. You can find one in the logs, run the command:
       -journalctl -b
The first place you should look is your YAML file. YAML can be picky about white space, you may need to change some settings in your text editor, particularly if you are running Windows as your host machine. You can use [Yaml Validator](https://www.yamllint.com/) to check the validity your yaml file.
#
### **Step 4: Create a new droplet**

Creating a droplet (or a virtual private server(VPS)) in Digital Ocean is a quick and straightforward process.

1. Click **Create** button located in the top right corner.
2. Click **Droplets** in the dropdown menu.
3. Choose **Region** that is geographically closest to you, if you actual location is not offred in the region options
![Regions](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/regions.png)
4. Select **Custom image** in the Choose an image section, and click **Add Image**, and upload the Arch Linux image that you have previously downloaded, for the purpose of this guide we are using a Custom Image. Other selection of OS images is available under the tab **OS**.
![Custom Image](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/custom.png)
5. Select **Biling Plan** that fits your need
6. Select Authentication method **SSH Key**
7. Click **+ Additional Options**, and Select **Add Inatilaztion cripts**, and **Paste** the content in the **Enter user data here** text box
![Additional Options](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/a.png)
8. Review additional options offered by Digital Ocean, and choose accordiign to your needs. Please note that all those sections are **Optional**
9. Click **Create Droplet** located in the bottom right corner.

*Check if everything went well. Click "[Your actual project name]" located in the top left corner under the dropdown menu **Projects**. When inside your projects under the tab __Resources__, you should see your newly created droplet. Refer to the image below*

![Droplet Created](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/Image%205.png)

### **Step 5:Connecting to your droplet via SSH**

1. **Copy** your dropltes public IP address

![IP address](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/ip%20address.png)

2. Open your Terminal
3. Type the command below, with you respective values
     
        ssh username@your_public_IP_address
4. Press Enter
5. You wiil see the a following meesage, Type **yes**

![Yes command](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/yes%20command.png)
6.Run the follwoing command, replace **username** with your actual username :
    
       ssh username

7. Your terminal should look similar to the image below. This is confirmation that you are connected to your droplet

![confirm](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/arch%20linux%20confiramtion.png)



**Congratulations, You have finished all the steps from this guide.**



##                                             References:

1. Digital Ocean documentation: https://docs.digitalocean.com/
2. Arch Linux Wiki: https://wiki.archlinux.org/
3. Cloud-init documentation: https://cloudinit.readthedocs.io/
4. Cloud-init initial server setup: https://www.digitalocean.com/community/tutorials/how-to-use-cloud-config-for-your-initial-server-setup
5. Digital Ocean Cloud-Init: https://docs.digitalocean.com/products/droplets/how-to/automate-setup-with-cloud-init/
6. Arch Linux Wiki (Cloud-init): https://wiki.archlinux.org/title/Cloud-init
7. YAML validator: https://www.yamllint.com/
8. Arch Linux Wiki (Wayland): https://wiki.archlinux.org/title/Wayland
9. What is SSH? https://www.cloudflare.com/learning/access-management/what-is-ssh/
10. How to create a Droplet? https://docs.digitalocean.com/products/droplets/how-to/create/
#