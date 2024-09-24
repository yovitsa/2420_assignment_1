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
- A personal computer with an operting system
- Access to the Internet
- Arch linux .qcow image (Download link may be found here: [Arch Linux Image](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/1545), Please downloand the **.qcow** which size is approximatley 500MB.


                          

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
1. Click on Settings located on the left side at your home page 
2. A new page will open, Click **Security** tab
3. New page will open, Click **Add SSH Key"" button which is located on the right side of the page 
 
- ![hi](2420_assignment_1\Image 3.png "Image")
4. In the **SSH Key Content** box use paste your **Public SSH Key**, give it a name of your choice. Good practice is to name your keys as something that you will remeber, for example
5. **"[MyProject]"** of course use the name of your project.
6. If everything went well you should have result similar to the image below.
7. If you are facing any issues, you may start the process all over again starting form the first step, that should fix the problem.

*Please note that before contiuing to the nexe Step 4, you have to ensure that you finish the previous steps in this guide*

**Step 4: Create a Project in your Digital Ocean account
1. Click **New Project** in the menu located on the left side of your screen.
2. Provide **Name** to your project
3. Select a **Purpose** of your project
4. Click **Create Project**

*Your Project should apper in the top left side menu, uner the dropdown menu __PROJECTS__* 

**Step 5: Create a new droplet**

Creating a droplet (or a virtual private server(VPS)) in Digital Ocean is a quick and straightforward process.
#
1. Click **Create** button in the top right corner,
2. Click **Droplets** in the dropdown menu
3. Choose  **Region**  that is geographically closest to you if you actual location is not offred in the region options
4. Select **Custom image** in the Choose an image section, and click **Add Image**, and upload the Arch lInux image that you have ppreviously downloaded.
5. Select **Biling Plan** that fits your need
6. Select Authentication method **SSH Key**
7. Review additional options offered by Digital Ocean, and choose accordigin to your needs, please note that all those sections are **Optional**
8. Click **Create Droplet** located in the bottom right corner.

*Check if everything went well, Click "[Your actual project name]" located in the top left corner under the dropdown menu __Projects__. When inside your projects under the tab __Resources__, you should see your newly created droplet. Refer to the image below*
##### Image of the droplet screenshot 5

##### Step 5 Create a new droplet

In order to create a new drolet we will need to install doctl on our machines,

#### What is doctl?
doctl (pronounced “dock-tul”, and short for “DigitalOcean Control”) is the official command-line interface for the DigitalOcean API.
#### What you can do with doctl?

doctl allows you to interact with your DigitalOcean resources from the command line of your local computer. Almost any action you can perform in the DigitalOcean Control Panel you can perform using doctl, such as creating and destroying Droplets, assigning reserved IP addresses, and updating cloud firewall settings. 

#### How to Install and Configure doctl
##### Step 1:Install doctl
Depending on your OS, you can follwo the guide below"
#### Installing doctl on **Windows**
1. Open **Power Shell**
2. Run the command below to download the most recent version of doctl:


    
       Invoke-WebRequest https://github.com/digitalocean/doctl/releases/download/v1.110.0/doctl-1 .110.0-windows-amd64.zip -OutFile ~\doctl-1.110.0-windows-amd64.zip
       
3. Next, extract th ebinary by running:
    
       Expand-Archive -Path ~\doctl-1.110.0-windows-amd64.zip
4. Open your **POwerShell** terminal with **Run as Adminstartor**, move the doctl binary into a dedicated directory and add it to your syste, path by running the following command:

       -New-Item -ItemType Directory $env:ProgramFiles\doctl\
       Move-Item -Path ~\doctl-1.110.0-windows-amd64\doctl.exe -Destination $env:ProgramFiles\doctl\
       [Environment]::SetEnvironmentVariable(
            "Path",
       [Environment]::GetEnvironmentVariable("Path",
       [EnvironmentVariableTarget]::Machine) + ";$env:ProgramFiles\doctl\",
       [EnvironmentVariableTarget]::Machine)
       $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
       
#### Step 2: Create an API token
  [Create a DigitalOcean API token](https://docs.digitalocean.com/reference/api/create-personal-access-token/ ) for your account with read and write access from the Applications & API page in the control panel. The token string is only displayed once, so save it in a safe place.

#### Step 3: Use the API token to grant account access to doctl
 Note
If you installed doctl using the Ubuntu Snap package, you may need to first create the user configuration directory if it does not exist yet by running mkdir ~/.config.
Use the API token to grant doctl access to your DigitalOcean account. Pass in the token string when prompted by doctl auth init, and give this authentication context a name.

    doctl auth init --context <NAME>

Authentication contexts let you switch between multiple authenticated accounts. You can repeat steps 2 and 3 to add other DigitalOcean accounts, then list and switch between authentication contexts:

      doctl auth list
      doctl auth switch --context <NAME>

Step 4: Validate that doctl is working
Now that doctl is authorized to use your account, try some test commands.

To confirm that you have successfully authorized doctl, review your account details by running:

     doctl account get

If successful, the output looks like:

    Email                      Droplet Limit    Email Verified    UUID                                        Status
    sammy@example.org          10               true              3a56c5e109736b50e823eaebca85708ca0e5087c    active

To confirm that you have successfully granted write access to doctl, create an Ubuntu 23.10 Droplet in the SFO2 region by running:

    doctl compute droplet create --region sfo2 --image ubuntu-23-10-x64 --size s-1vcpu-1gb <DROPLET-NAME>

You can get a full list of available Droplet images by running doctl compute image list. The output of that command includes an ID column with the new Droplet’s ID. For example:

    ID           Name            Public IPv4    Private IPv4    Public IPv6    Memory    VCPUs    Disk    Region    Image                       Status    Tags    Features    Volumes
    187949338    droplet-name    1024      1        25      sfo2      Ubuntu 18.04.3 (LTS) x64  new

Use that value to delete the Droplet by running:

    doctl compute droplet delete <DROPLET-ID>

When prompted, type **y** to confirm that you would like to delete the Droplet.
       
#### Installing doctl on Linux and MacOS
Visit the [Releases page](https://github.com/digitalocean/doctl/releases "Releases page"). for the doctl GitHub project and find the appropriate archive for your operating system and architecture. Download the archive from your browser or copy its URL and retrieve it to your home directory with wget.

For example, to download the most recent version of doctl for Linux with wget, run:

     cd ~
     wget https://github.com/digitalocean/doctl/releases/download/v1.110.0/doctl-1.110.0-linux-amd64.tar.gz

Next, extract the binary. For example, to do so using tar, run:

     tar xf ~/doctl-1.110.0-linux-amd64.tar.gz

Finally, move the doctl binary into your path by running:

    sudo mv ~/doctl /usr/local/bin
 
Step 2: Create an API token
[Create a DigitalOcean API token](https://docs.digitalocean.com/reference/api/create-personal-access-token/ ) for your account with read and write access from the Applications & API page in the control panel. The token string is only displayed once, so save it in a safe place.

Step 3: Use the API token to grant account access to doctl
Note
If you installed doctl using the Ubuntu Snap package, you may need to first create the user configuration directory if it does not exist yet by running 
        mkdir ~/.config.
Use the API token to grant doctl access to your DigitalOcean account. Pass in the token string when prompted by doctl auth init, and give this authentication context a name.

        doctl auth init --context <NAME>

Authentication contexts let you switch between multiple authenticated accounts. You can repeat steps 2 and 3 to add other DigitalOcean accounts, then list and switch between authentication contexts:

        doctl auth list
        doctl auth switch --context <NAME>

Step 4: Validate that doctl is working
Now that doctl is authorized to use your account, try some test commands.

To confirm that you have successfully authorized doctl, review your account details by running:

        doctl account get

If successful, the output looks like:

     Email                      Droplet Limit    Email Verified    UUID                    Status sammy@example.org              10           true          3a56c5e109736b50e823eaebca  active     
Copy
To confirm that you have successfully granted write access to doctl, create an Ubuntu 23.10 Droplet in the SFO2 region by running:

        doctl compute droplet create --region sfo2 --image ubuntu-23-10-x64 --size s-1vcpu-1gb <DROPLET-NAME>

You can get a full list of available Droplet images by running doctl compute image list. The output of that command includes an ID column with the new Droplet’s ID. For example:

    ID           Name            Public IPv4    Private IPv4    Public IPv6    Memory    VCPUs    Disk    Region    Image                       Status    Tags    Features    Volumes
    187949338    droplet-name                                                  1024      1        25      sfo2      Ubuntu 18.04.3 (LTS) x64    new

Use that value to delete the Droplet by running:

    doctl compute droplet delete <DROPLET-ID>

When prompted, type **y** to confirm that you would like to delete the Droplet.

### Creating a droplet with  Arch linux OS using doctl
We will create a our custom image droplet with Arch Linux OS.
Run the following command in order to get the list of images:
        
    doctl compute image list

You will see a list similar to image below:
## image 6 
Step 1:Image ID:

AS you may notice the Arch Linux image does not have an im age Slug, 
In order to install Arch LInnux we will us the Arch Linux ID which you may find on the image below located in **red** swqaure

### image 7

Step 2: Upload Your Custom Image by running the command below:
        
        doctl compute image create --image-url <URL_OF>YOUR>IMAGE> --region <YOUR_REGION>

Replace <URL_OF_YOUR_IMAGE> with URL where Arch LInux is hosted. You can find it here [Arch Linux Image](https://gitlab.archlinux.org/archlinux/arch-boxes/-/package_files/7529/download)

Step 3: Create the Droplet:
Run the command below to create a droplet, replace your image ID with the ID from step one.

        doctl compute droplet create <DROPLET NAME> --region <YOUR_REGION> --image <IMAGE_ID> --size <DROPLET_SIZE> --ssh-keys <YOUR_SSH_KEYS>
        
Replace <DROPLET NAME>, <YOUR_REGION>, <IMAGE_ID>, <DROPLET_SIZE>, <YOUR_SSH_KEYS>, with your desired values.

Here is an example of the command:
    
      doctl compute droplet create Arch --region sfo3 --image 165064171 --size s-1vcpu-1gb --ssh-keys 1234567891234569
      
Run the command:
        
        doctl compute droplet list

Your output should be similar to this one:
###image 8

![Output result](https://github.com/yovitsa/2420_assignment_1/blob/main/assets/image%208.png "Image")


## 