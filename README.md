# make secure SFTP connection for remote file exchanging

Making SFTP connection between two machines could be very useful as it opens a wide range of possibilities. You can see all files on your PC, can remotly make a new file, edit existing files or download any. 
This is a guide how to establish a test SFTP connection between two Ubuntu virtual machines. To make this possible I'll set up an SSH connection first cause I need to transfer encryption keys for this purpose.

In my case (I'm using VirtualBox 7.0.18) to make 2 virtual machines see each other I need to make custom NAT network at path file-tools-networkTool-create NATnetwork and enter IP address, eg 192.168.100.0/24.

<p align="center">
<img src="https://github.com/user-attachments/assets/485143a7-3503-4c9a-aa9c-7691a04ff552">
</p>

Use `ifconfig` on both machines to record IP addresses. 

<p align="center">
<img src="https://github.com/user-attachments/assets/a0149dbf-642c-4797-a923-b54c12a0e40b">
</p>

First thing to do is to create a public-private key pair with command `ssh-keygen` and name your key. This will create a file in your home directory with a name `*your key name*.pub` which means public key.

<p align="center">
<img src="https://github.com/user-attachments/assets/b191b7b8-8f4c-42db-8fca-7d8cbbeb997f">
</p>

Once file with your public key is created you need to transfer this file to your 2nd machine with this command: <br>`sudo ssh-copy-id -i *your public key file name* *2nd machine username*@*2nd machine IP address*`.

<p align="center">
<img src="https://github.com/user-attachments/assets/1a2a5a45-cdf4-4465-a138-17635d5624e4">
</p>

Once SSH public key was transfered it could be found in a file named authorized_keys. Now the content of this file is the same to the file with a public key on a 1st machine.

<p align="center">
<img src="https://github.com/user-attachments/assets/1615e380-42ac-4ca8-ba2a-bd8b5de2e17f">
</p>

To be sure that SSH connection will work fine check for an sshd process with this command: `sudo netstat -anp | grep sshd`. There should be an sshd process which sygnals that openssh is listening for incoming connections.

<p align="center">
<img src="https://github.com/user-attachments/assets/869f5c26-fd97-4eb6-bfec-6624e463c559">
</p>

Establish an SSH connection first to check if it's working fine, then establish an SFTP connection to get file fetching possibility. The following commands: `ssh *username*@*IP address*` - SSH, `sftp *username*@*IP address*` - SFTP.

<p align="center">
<img src="https://github.com/user-attachments/assets/b9b6d076-d089-4b32-9c3d-56b242fd7086">
</p>

On a virtual machine to which you had been connected you can ran a command `sudo netstat -anp | grep sshd` and see that your connection is fine.

<p align="center">
<img src="https://github.com/user-attachments/assets/6d360e19-465a-466c-a278-736f483df316">
</p>

Once connected via SFTP we could easily navigate through remote machine and iteract with files. All your commands that you'll run will be directed and executed on a connected machine. To make your commands execute on a local machine it's required to use "!" or in some cases "l" (letter "L" not pipe) prefix. Use "l" prefix without any space to the next command same is to "!" prefix. You can also type single "!" and all next commands will be related to local machine and for coming back to connected machine need to type `exit` as well as to stop an established connection. To download any file simply run `get *path to the file*`.

<p align="center">
<img src="https://github.com/user-attachments/assets/9bbe7933-2579-48c9-8795-d6bb22757887">
</p>
