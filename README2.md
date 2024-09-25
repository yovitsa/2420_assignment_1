# 2420 Assignment 1
###### Student Name: Jovica Kuzmanovic
###### Student ID: A01339297
#
#
#
# Work in progess 

This guide assume that you already have digital ocean account and that you psses the basic computer knoweldge, lke for example oipning a terminal on your operating sytem(OS). If you do not psses any of those this guide is not for you
# Requierments
- Digital Ocean Account
- A personal computer with an operating system
- Access to the Internet
- Arch linux .qcow image (Download link may be found here: [Arch Linux Image](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/1545) , Please downloand the **.qcow** which size is approximatley 500MB.
#

                          

#### What is Secure Shell Protocol(SSH)?
- SSH is a method for securely sending commands to a computer over an unsecured network. The SSH uses cryptography to authenticate and encrypt connections between devices.
- SSH is secure because it incorporated encryption and authentication via process called public key cryptography. Public key cryptography is the way to encrypt the data, or sign data, with two different keys, one key is public, the other one is private kept by the owner.

#### What does SSH do? 
- SSH can do remote encrypted connections  SSH sets up a connection between a users device and a faraway machine, often a server, It uses encryption to scramble the data that traverses the connection.
- SSH also has abillty to tunnel, tunneling is a method for moving packets across a network using a protocol or path they would not ordinarily be able to use, Tunneling works by wrapping data packets with additional information called headers to change their destination.

#### When do we use SSH?
- SSH is often used for controlling servers remotely, for managing infrastructure, and for transferring files.
#
#### Creating an (SSH) key pair on your local machine
#
**Step 1: Create an SSH key pair**
#
1. Open the Terminal on your machine
2. If you are using Windows as your OS you may need to create .ssh direcotory in your home directory first
3. (Tilda) ~ == home directory
    
Create a SSH key pair on **Linux** or **MacOS** type the following command:
        
         ssh.keygen -t ed25519 -f ~/.ssh/do-key -C "youremail@address"
         
On **Windows** machine type the following command:
    
         ssh-keygen -t ed25519 -f C:\Users\your-user-name\.ssh\do-key -C "youremail@email.com"

         
The command above will create a two text files in your .ssh directory

**"do-key"** or **"do2-key"**    This is your private key
**"do-key.pub"** or **"do2-key"**  This is the public key, this key will be copied to a server.
#      
**Step 2: Copying your Public Key**
#
Our keys are just a plain text files, on order to make sense of those files, we will use our terminal.
Copying our public key is a straight forward process,  depending on your OS, please **COPY** the command into your terminal.

For **Windows** users:


      Get-Content C:\Users\your-user-name\.ssh\do-key.pub | Set-Clipboard

      
For **MacOS** users:

    
       pbcopy < ~/.ssh/do-key.pub
 
For **Linux**  users it will depend on the type of your distribution, [lease refer to the doucmetation of your distribution, here are some of the popular commands, note that the first command requires **Wayland** text editor:
    
       wl-copy < ~/.ssh/do-key.pub
#
       xclip -selection clipboard < ~/.ssh/do-key.pub

#  
**Step 3: Adding your public key to Digital Ocean**
#
1. Click on Settings located on the left side at your home page 
2. A new page will open, Click **Security** tab
3. New page will open, Click **Add SSH Key"" button which is located on the right side of the page 
 
4. ![hi](2420_assignment_1\Image 3.png "Image")
5. In the **SSH Key Content** box use paste your **Public SSH Key**, give it a name of your choice. Good practice is to name your keys as something that you will remeber, for example
6. **"[MyProject]"** of course use the name of your project.
7. If everything went well you should have result similar to the image below.
8. If you are facing any issues, you may start the process all over again starting form the first step, that should fix the problem.

*Please note that before contiuing to the nexe Step 4, you have to ensure that you finish the previous steps in this guide*
#
**Step 4: Create a Project in your Digital Ocean account**
#
 1. Click **New Project** in the menu located on the left side of your screen.
 2. Provide **Name** to your project
 3. Select a **Purpose** of your project
 4. Click **Create Project**

*Your Project should apper in the top left side menu, uner the dropdown menu __PROJECTS__* 
#
#
*Before we create a cloud-init file, we should learn more about cloud-init*
#
#### What is cloud init?
Cloud-init allows us to setup a server with some initial configuration. In order to configure cloud-init we shoudl create a configuration file.
One of the better pracitces is to create a YAML file.

#### What is YAML file?
[YAML](https://yaml.org/) is a human-readable data serialization language often used for configuration files, data storage, and inter-process communication. It's designed to be more readable than JSON and XML.

#### What does cloud-init do?
Cloud-init can handle a range of tasks that normally happen when a new instance is created. It’s responsible for activities like setting the hostname, configuring network interfaces, creating user accounts, and even running scripts. This streamlines the deployment process; your cloud instances will all be automatically configured in the same way, which reduces the chance to introduce human error.

#### How does cloud-init work?
The operation of cloud-init broadly takes place in two separate phases during the boot process. The first phase is during the early (local) boot stage, before networking has been enabled. The second is during the late boot stages, after cloud-init has applied the networking configuration.

**Step 6: Create configuration file cloud-init**

1. Run teh commnad below in order ot confirm that clound-init is running:

       systemctl status cloud-init
2. Create  cloud-init configuration in your .ssh folder. Yamml file needs to have an **.yml** extension
    **example: cloud-init.yml **
3. Edit cloud-init file in the text editor of your choice, you may use notepad, Visual Studio Code, or any other program i whihc you can edit text.
4. Copy the example below and paste into your cloud-init configuration file, and make changes as instructed. 
    
        #cloud-config
        users:
        -name: user-name #change me
        -primary_group: group-name #change me
        -groups: wheel
        -shell: /bin/bash
        -sudo: ['ALL=(ALL) NOPASSWD:ALL']
        -ssh-authorized-keys: #paste here you public ssh key
            - ssh-ed25519 ...

        packages:
        - ripgrep
        - rsync
        - neovim
        - fd
        - less
        - man-db
        - bash-completion
        - tmux

        disable_root: true

#### Troubleshooting a cloud config file

*If the configuration in your file is not being applied you probably won't see an error message. You can find one in the logs (journalctl -b).
The first place you should look is your YAML file. YAML is picky about white space, you may need to change some settings in your text editor, particularly if you are running Windows as your host machine. You can use [Yaml Validator](https://www.yamllint.com/) to check your yaml file*
#
**Step 5: Create a new droplet**
#
Creating a droplet (or a virtual private server(VPS)) in Digital Ocean is a quick and straightforward process.

1.Click **Create** button in the top right corner,
2.Click **Droplets** in the dropdown menu
3.Choose  **Region**  that is geographically closest to you if you actual location is not offred in the region options
4.Select **Custom image** in the Choose an image section, and click **Add Image**, and upload the Arch lInux image that you have ppreviously downloaded.
5.Select **Biling Plan** that fits your need
6.Select Authentication method **SSH Key**
7.Click **+ Additional Options**, and Select **Add Inatilaztion cripts**, and **Paste** teh conect in the **Enter your data here** text box
[Additional Options](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/a.png)
8.Paste teh content of your cloud-init.yml file in the data box as the image below
[text box]()
7.Review additional options offered by Digital Ocean, and choose accordigin to your needs, please note that all those sections are **Optional**
8.Click **Create Droplet** located in the bottom right corner.

*Check if everything went well, Click "[Your actual project name]" located in the top left corner under the dropdown menu __Projects__. When inside your projects under the tab __Resources__, you should see your newly created droplet. Refer to the image below*
![Droplet Created](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/Image%205.png)

#
**Step 7: Adding your cloud configuration to Digital Ocean**
#

1. Opem teh 

