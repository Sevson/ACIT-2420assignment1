"# ACIT-2420assignment1" 

# Arch Linux Setup with Cloud-init
1. **SSH Key Generation**: 
   To generate the SSH keys on your local machine you first need a .ssh directory in your home directory.
   once you have a ssh directory enter the following into your command terminal
     ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address"
     ssh-keygen -t ed25519 -f C:\Users\your-user-name\.ssh\do-key -C "youremail@email.com"

   The command above will create two plain text files in the .ssh directory.
     "do-key" This is your private key
     "do-key.pub" This is your public key. The public key is the one that you copy to your server.

   After you have created this key pair you need to add your public key to your DigitalOcean account.

2. **Add SSH key to your Digital Ocean Account**
    To get the contents of the recently creating file into your clip board enter the following into powershell.
      Get-Content C:\Users\your-user-name\.ssh\do-key.pub | Set-Clipboard

   When you have the contents in your clip board go to DigitalOcean console and navigate to the settings. In settings go to     the Security Tab click the "Add SSH Key" button. Paste the contents of your public key into the "SSH Key content" box and    give your key a name.
   
3. **Creating a Droplet**:
   Click the create button in the top right, and select "Droplets". From there select your droplet settings

4. **Using Cloud-init**:
     1. Install QEMU
     2. Create a Temporary directory with
          $ mkdir temp
          $  cd temp
     3. Download a Cloud image
          Download wget with: brew install wge
          Paste the following into terminal: wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
     4. Define User Data
          Create the user-data file with:
            $ cat << EOF > user-data
            #cloud-config
            password: password
            chpasswd:
            expire: False
    5.  Define Meta Data
          cat << EOF > meta-data
          instance-id: someid/somehostname

        Define our vendor data
        # Inside the temp directory paste this in to speed up the retry wait time
        Paste this: touch vendor-data
        
     6. Stat an Ad Hoc IMDS Web Server
          In the terminal app press cmd+n
          Navigate to the temp directory : cd temp

     7. Log In
     8. Close the Environment
        1. Exit the QEMU Virtaul Machine
           Press: Ctrl + A
           Then press: x

        2. Stop the Python HTTP Server
           Go to the terminal where your Started the Python HTTP server and stop it by pressing: Ctrl + C

#cloud-config

# Create a new user with sudo privileges
users:
  - name: git
    ssh-authorized-keys:
      - ssh-rsa YOUR_SSH_PUBLIC_KEY
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    groups: users, sudo

# Disable root access via SSH
ssh_pwauth: false
disable_root: true

# Install initial packages
packages:
  - git
  - base-devel
  - vim  # Example package

# Additional commands
runcmd:
  - echo "Setup completed!"
