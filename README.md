# Linux-Assignment-1
Digital Ocean Tutorial

<h2>Creating SSH Keys </h2>

<p>Creating an SSH key pair provides an extra layer of security as it is a more secure authentication method. It will also simplify the process of accessing your server. </p>

>Note: This tutorial assumes that the user is using Windows Powershell.

- Start in your home directory using ```~```
- Create a .ssh directory in your home directory


**Type** the following command into the terminal: 

```ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address```

1. **ssh-keygen** is the command used to create the public/private keys
2. **-t** = the type of encryption used for this key
3. **-f** = filename and where the file is located
4. **-C** = comment (attaches a comment to the key)
5. **do-key** = the name for the generated key
>dokey.pub will be your public key that you copy to your server.

<h3> Copying SSH Keys to Clipboard</h3>

After creating the key pair, you can **type** the following into the terminal to get the contents directly:

```Get-Content C:\Users\your-user-name.ssh\do-key.pub | Set-Clipboard```

1. Open DigitalOcean **"Settings"** in the left menu
2. Click **"Security"**
3. Click **"Add SSH Key"** button
4. Paste the public key into the **"SSH Key content"** box 
5. Give the key a name

## Adding the Custom Arch Linux Image ##
We will be adding a custom image to deploy the server with a pre-configured environment. 
- Download the [Arch Linux](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/) image
- Make sure it is the most recent **"clowdimg"**

![screenshot](https://cdn.discordapp.com/attachments/1194392650858643528/1287573588051886211/image.png?ex=66f209d9&is=66f0b859&hm=cb2b176201375b90e4282d9264ed4ea266d31ed6f336f5254757603403912978&)