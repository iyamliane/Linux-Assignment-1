# DigitalOcean Server Setup #

>***Note:*** This tutorial assumes that you are using Windows Powershell.


**What is DigitalOcean?**

DigitalOcean provides cloud computing services (object storage, databases, block storage etc.) allowing users (developers, startups, businesses) to deploy their applications. You can create and deploy your own virtual private servers also known as Droplets. 

This tutorial will walk you through the process of:
  1. Creating your SSH key pair
  2. Uploading a custom Arch Linux image
  3. Creating a new droplet
  4. Cloud-init setup
  5. Connecting to your server

<h2>Creating SSH Keys </h2>

<p>Creating an SSH key pair provides an extra layer of security as it ensures your authentication method is secure. It will also simplify the process of accessing your server. We will be using the Windows terminal to complete the following steps. </p>


- Start in your home directory by using this command:  ``` cd ~```
  - ```cd``` = change directory
- Create a .ssh directory in your home directory by using this command: ```mkdir .ssh```
  - ```mkdir``` = make directory
- Type the following command: 

```ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address```


1. **ssh-keygen** = the command used to create the public/private keys
2. **-t** = the type of encryption used for this key
3. **-f** = filename and where the file is located
4. **-C** = comment (attaches a comment to the key)
5. **do-key** = the name for the generated key
>dokey.pub will be your public key that you copy to your server.

<h3> Copying SSH Keys to Clipboard</h3>

Copying your SSH public key to the clipboard will allow you to easily paste it into the appropriate configuration file or service without needing to navigate through directories or files.

After creating the key pair, you can type the following into the terminal to get the contents directly:

```Get-Content C:\Users\your-user-name.ssh\do-key.pub | Set-Clipboard```

You want to open up DigitalOcean in your web browser. Complete the following steps next:

1. Navigate to **"Settings"** in the left menu under **"Manage"**

![settings](/attachments/Settings.png) 

2. Click **"Security"**

![security](/attachments/security.png)

3. Click **"Add SSH Key"** button

![add-sshkey](/attachments/add-sshkey.png)

4. Paste the public key into the **"SSH Key content"** box 

![ssh-key-pase](/attachments/ssh-key-paste.png)

5. Give the key a name

## Uploading the Custom Arch Linux Image ##
A custom image specifically refers to a pre-configured template which includes an operating system, applications, and settings.

You will be adding a custom image to deploy the server with a pre-configured environment as DigitalOcean does not have a default image for Arch. 
- Download the [Arch Linux](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/) image by clicking the link: https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/

- Click on 'images'

![images](/attachments/arch-images.png)

- Make sure it is the most recent **"clowdimg"** ending in **".qcow2"**

![qcow2](/attachments/qcow2.png)

>**qcow2** is a format that is commonly used for disk images. It is great for virtual machines/backups and supports a variety of useful features (compression, copy-on-write).

Once you have downloaded the image, open up DigitalOcean and complete the following steps:
1. Look for **"Backups & Snapshots"** in the left menu under **"Manage"**

2. Inside this menu, select **"Custom Images"**

3. Click the blue **"Upload Image"** button

![upload-image](/attachments/upload.png)

4. Add the custom image that you downloaded earlier

![example](/attachments/example.png)

## Creating a new Droplet ##
This refers to the process of adding a virtual server(instance) in the cloud. A droplet is basically a virtual machine (VM) in the DigitalOcean infrastructure. You are able to choose from a wide variety of operating systems but in our case we will be using Arch. 

You want to open up DigitalOcean in your web browser. Complete the following steps next:

1. Click the green **"Create"** button in the upper right corner

![create](/attachments/create.png)

2. Click **"Droplets"** in the dropdown menu

3. Choose **"Datacenter 3 / SFO3"** 

![SFO](/attachments//SFO.png)

4. Click **"Custom Images"** and add the Arch image

![custom-image](/attachments/custom-upload.png)

5. Choose **"Basic"** for size

![basic](/attachments/basic.png
)

6. Choose **"7/mo"** for CPU options under **"Premium AMD"**

![basic](/attachments/amd.png)


7. Choose the previously added SSH key for **"Authentication Method"**

## cloud-init Setup ##
Using the cloud-init allows you to automate the setup of your server when it is created in the cloud. This is particularly useful if you wanted to update/install packages for multiple servers.

> The simplest way to configure a server with cloud-init is by using a config (**YAML**) file.

- After adding your SSH key, click on **"Advanced Options"**

![advanced](/attachments/advanced-options.png)

- Click **"Add initialization scripts(free)"** <br>
Copy the following into the text box below:
```
users:
  - name: user-name 
    primary_group: user-group 
    groups: wheel
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-ed25519 your-ssh-public-key # change this

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
```
![screenshot](/attachments/config-file.png)


>***Note:*** change the ```name``` to a username you want for the new user, ```primary group``` refers to the main group associated with the user. You can change ```user-group``` to ```users```.

## Connecting to your Server ##
Once your droplet is created and the configuration has been setup, you can connect to it with SSH.
1. Type the following command into the terminal:<br>
```ssh -i .ssh/do-key arch@your_droplet_ip_address```

2. replace ```your_droplet_ip``` with your actual droplet IP address. 
- ```-i``` is the path to the private key file
- arch = your username
> ***Note:*** to get the droplet IP address, go to your **"Droplets"** and hover over **"ipv4"** and click **"Copy"** 

3. To exit your SSH connect, type ```exit``` and press **"Enter"**

To make logging in your server easier, you can use this method of creating a config file in your .ssh directory so that you do not need to use a longer command in the terminal while logging in.

1. Open the terminal and run the following command:

   ```notepad your-user\.ssh\config```

2. Inside of the configuration file note, add the following: 
  ```
    Host droplet-name
    HostName XXX.XXX.XXX.XXX
    User your-user
    IdentityFile ~/.ssh/id_rsa
```
> ***Note:*** Make sure to replace ```droplet-name``` with the alias you will connect to the server with (e.g., arch), replace ```HostName``` with your droplet's IP address and replace ```your-user``` with the username on the server.

3. Save notepad, and close the terminal

4. Open the terminal again and type:

```ssh droplet-name``` making sure to replace ```droplet-name``` with the alias you created to connect to the server with. 

## References ##

1. https://docs.digitalocean.com/products/droplets/how-to/automate-setup-with-cloud-init/
2. https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/
3. https://docs.digitalocean.com/products/droplets/how-to/add-ssh-keys/
4. https://gitlab.com/cit2420/2420-notes-f24/-/blob/main/2420-notes/week-two.md
5. https://gitlab.com/cit2420/2420-notes-f24/-/blob/main/2420-notes/week-three.md
6. https://www.techtarget.com/searchcloudcomputing/definition/DigitalOcean
