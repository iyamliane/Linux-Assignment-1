# Linux-Assignment-1
Digital Ocean Tutorial

<h2>Creating SSH Keys on your Local Machine</h2>

<p>Creating an SSH key pair provides an extra layer of security as it is a more secure authentication method. It will also simplify the process of accessing your server. </p>

- Start in your home directory using ```~```
- Create a .ssh directory in your home directory


> **Type** the following command into the terminal: 

```ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address```

1. **ssh-keygen** is the command used to create the public/private keys
2. **-t** = the type of encryption used for this key
3. **-f** = filename and where the file is located
4. **-C** = comment (attaches a comment to the key)
5. **do-key** = the name for the generated key
>dokey.pub will be your public key that you copy to your server.