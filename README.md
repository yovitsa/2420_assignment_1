# 2420 Assignment 1
###### Student Name: Jovica Kuzmanovic
###### Student ID: A01339297
#
#
#
# Work in progess 

This guide assume that you already have digital ocean account and that you psses the basic computer knoweldge, lke for example oipning a terminal on your operating sytem(OS). If you do not psses any of those this guide is not for you
# Requierments
- Digital OCean Account
- A personal computer with an operting system
- Access to the Internet
- Arch linux .qcow image (Download link may be found here: [Arch Linux Image](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/1545) ), Please downloand the **.qcow"" which size is approximatley 500MB.


                          

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
- Open the Terminal on your machine
- If you are using Windows as your OS you may need to create .ssh direcotory in your home directory first
- (Tilda) ~ == home directory
    
Create a SSH key pair on **Linux** or **MacOS** type the following command:
        
         ssh.keygen -t ed25519 -f ~/.ssh/do-key -C "youremail@address"
         
On **Windows** machine type the following command:
    
         ssh-keygen -t ed25519 -f C:\Users\your-user-name\.ssh\do-key -C "youremail@email.com"

         
The command above will create a two text files in your .ssh directory

**"do-key"** or **"do2-key"**    This is your private key
**"do-key.pub"** or **"do2-key"**  This is the public key, this key will be copied to a server.
      
**Step 2: Copying your Public Key**

Our keys are just a plain text files, on order to make sense of those files, we will use our terminal.
Copying our public key is a straight forward process,  depending on your OS, please **COPY** the command into your terminal.

For **Windows** users:


      Get-Content C:\Users\your-user-name\.ssh\do-key.pub | Set-Clipboard

      
For **MacOS** users:

    
       pbcopy < ~/.ssh/do-key.pub
 
For **Linux**  users it will depend on the type of your distribution, [lease refer to the doucmetation of your distribution, here are some of the popular commands, note that the first command requires **Wayland** text editor:
    
    wl-copy < ~/.ssh/do-key.pub
    xclip -selection clipboard < ~/.ssh/do-key.pub
    
**Step 3: Adding your public key to Digital Ocean**
- Click on Settings located on the left side at your home page 
- A new page will open, Click **Security** tab
- New page will open, Click **Add SSH Key"" button which is located on the right side of the page 
 
     ![hi](2420_assignment_1\Image 3.png "Image")
- In the **SSH Key Content** box use paste your **Public SSH Key**, give it a name of your choice. Good practice is to name your keys as something that you will remeber, for example
- **"[MyProject]"** of course use the name of your project.
- If everything went well you should have result similar to the image below.
- If you are facing any issues, you may start the process all over again starting form the first step, that should fix the problem.

**Step 4

## 