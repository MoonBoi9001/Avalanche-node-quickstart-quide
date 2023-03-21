## Disclaimer:

Please refer to the [README.md](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide#disclaimer) file for the full disclaimer related to this guide.

## Prerequisites

- A reliable internet connection.
- A minimum of 2,000 AVAX tokens for staking (accurate at the time of writing; subject to change).
- Basic understanding of command-line interfaces.

### 1. Download and Install CMDER

Download and install the latest full version of CMDER, a Linux terminal emulator for Windows, which enables you to log into your server: https://cmder.app/  

### 2. Create a GitHub Account

Create an account on GitHub if you don't already have one: https://github.com/signup?source=login

### 3. Generate an SSH Keypair

Generate a new SSH keypair to securely log into your server. If you already have a keypair on your PC, you can skip this step.

#### 3.1 Check for Existing Keypair

Go to the following directory on your Windows PC:
C:/Users/{your_username}/.ssh
If you see files named `id_rsa` and `id_rsa.pub`, you already have a keypair that you can use.

#### 3.2 Create a New SSH Keypair

If you don't have any files or the `.ssh` directory doesn't exist, follow these steps:

1. Create a suitable folder (e.g., `hetzner_germany_avax_server_AX41-NVME`) in this directory: `C:/Users/{your_username}/.ssh` for saving your SSH keys inside.
2. Open the newly created folder.
3. Launch Command Prompt by typing `cmd` into Windows search and pressing Enter. This will launch a terminal window.
4. Type `ssh-keygen` into the terminal window.
5. Enter a file to save your SSH keypair, e.g., `C:/Users/{your_username}/.ssh/{your_folder_name}/id_rsa`
6. Enter a secure passphrase to encrypt your SSH key on your Windows device. Choose a passphrase that is at least 12 characters long, with a mix of uppercase and lowercase letters, numbers, and special characters. This passphrase is crucial to protect your SSH key, and you MUST remember it!
7. Confirm that the newly created keypair is saved into the new folder you created previously.
8. Go to the following directory on your Windows PC: `C:/Users/{your_username}/.ssh/{your_folder_name}`
9. Copy the file named `id_rsa` (not the one that says `id_rsa.pub`).
10. Navigate to the following directory on your Windows PC: `C:/Users/{your_username}/.ssh`
11. Paste a copy of your `id_rsa` file into this directory.
12. Go back to the original directory: `C:/Users/{your_username}/.ssh/{your_folder_name}`
13. Open the file named `id_rsa.pub` in Notepad and then minimize Notepad. You will come back to it later. You can close the File Explorer now.

# Guide starts here if you have completed the prerequsites 

## Registering your server

#### 1. Purchase AX41-NVME on Hetzner

Start by purchasing an AX41-NVME server on Hetzner in Germany or Finland, keeping in mind any one-time setup fees: [Hetzner AX41-NVME Configurator](https://www.hetzner.com/dedicated-rootserver/ax41-nvme/configurator#/)
- Keep the option "Primary IPv4" checked and selected, as you will need it.
- Select the latest version of Ubuntu LTS (at the time of writing, this is Ubuntu 22.04.1 LTS (base)).
- Optionally consider upgrading to ECC RAM near the bottom of the order page (not required).

#### 2. Complete Your Order

Click "Order" to proceed to the checkout page. On the checkout page, you will have the option to log in to your server using either a password or a public key. Choose the public key option.
- If you followed the earlier part of this guide, your Notepad with the public key should be open already. Copy your public key and paste it into the "Key Data" field.
- If you're not sure where to find your public key, go to the following directory: `C:/Users/{your_username}/.ssh` and find the file named `id_rsa.pub`.

Save and complete your order. You may have to wait up to a few hours until the server is available.

## Logging into your server

Once your server has finished being configured, it should appear in the `Server ID` section of the Hetzner Robot dashboard (https://robot.hetzner.com/server). For example, you might see `AX41-NVMe #1871660`.

1. Click on your Server's ID and then copy the IP address. You can make a note of the IP somewhere for future reference, or you can always find it using the link provided above.
2. Launch `Cmder.exe`, and type `ssh root@{IP}`. Make sure to replace "`{IP}`" with the IP address of your server that you copied from the link above.
3. Your Linux terminal emulator will now connect to your server and prompt you to trust the key fingerprint. This will only be asked once, during the initial setup. Type `yes` and press Enter.
4. Enter your secure passphrase for your SSH keypair when prompted.

## Beginning configuration of your server.

Now that you have successfully logged into your server, you're ready to begin configuration. The following steps will guide you through setting up your node. Make sure to follow the steps sequentially, as entering the code out of order may result in a failure to set up the node. It's recommended that you read the explanation of the code before entering it into your server so you are aware of what each line performs.

### Step 1: Update the package list and installed packages

1. `sudo apt update && sudo apt upgrade`. After entering, press `Y` (to continue) and may also need to press `Enter` on your keyboard to select okay a few times during the configuration process. You don't need to restart any more processes than recommended; simply press `Enter`.

### Step 2: Create a non root user account

2. `sudo adduser mainuser`. After entering, set a new password. It doesn't really matter what you set it to, as you won't use it. Just make it sufficiently complex so it's not hacked before you disable password login (but make sure to remember it). Press `Enter` a few times to set default new user values. Then press `Y` to show information is correct.

### Step 3: Add the non root user to the sudo group

3. `usermod -aG sudo mainuser`

### Step 4: Log out of your server and log back in as the new user

4. Close Cmder, then relaunch it and enter the following command, replacing `{ip}` with your server's IP address `ssh mainuser@{ip}`. Press `Enter` to skip the SSH passphrase, then enter your newly created main user password (the one you were supposed to remember). If you take too long, it may say "Connection closed by '{ip}' port 22"; just try again, but do it faster this time.

### Step 5: Configure SSH key-based authentication for the new user

5. 1. First make a new SSH directory in your server `mkdir -p ~/.ssh`. Then, from your windows file explorer, open the `id_rsa.pub` file in your SSH directory (`C:/Users/{enter your username here}/.ssh`) using Notepad, and then copy the entire contents of the file. Once the public key string is copied, go back to Cmder where you are logged into your server. Type `echo {public_key_string} >> ~/.ssh/authorized_keys` but make sure to replace `{public_key_string}` with your entire SSH public key string that you copied from Notepad. After entering, you will need to accept the warning message that the long text may make the console non-responsive. 

    2. Verify the above line worked with the following command, which will show your key: `nano /home/mainuser/.ssh/authorized_keys` If you can see your public SSH key then you have completed the step successfully. Close the Nano text editor by pressing `CTRL` + `X`.

### Step 6: Secure the SSH configuration

6. Secure your SSH configuration by setting the appropriate permissions, ownership, and updating the configuration file.

    `chmod -R go= ~/.ssh`
    `chown -R mainuser:mainuser ~/.ssh`
        
    Open the SSH configuration file with nano:
    
        `sudo nano /etc/ssh/sshd_config`
        
    Enter your user password (the one you were supposed to remember).

    Find and change the following lines:

        PermitRootLogin yes
        PubkeyAuthentication yes
        PasswordAuthentication no
        PermitEmptyPasswords yes

    Then press `Ctrl` + `x`, press `Y` to save the modified buffer, and then press `Enter` to write the file.

    `sudo systemctl restart ssh`

### Step 7: Set up the firewall

`sudo apt install ufw`
`sudo ufw allow 2222`
`sudo ufw allow 9651`
`sudo ufw enable`

# sudo systemctl restart ssh
# sudo apt install ufw
# sudo ufw allow ssh
# sudo ufw allow 9651
# sudo ufw enable
#### Press y to proceed.
# sudo ufw status numbered
#### You should see that ssh is allowed in from anywhere on port 22. And the Port 9651 is open and accessible.
# sudo visudo
#### Enter the following line:
# mainuser ALL=(ALL) NOPASSWD:ALL
#### I think you can enter the above line anywhere, but i put it under the "# user privilege specification" section.
#### Save the file with Crtl + x, then press Y, then enter to save the modified buffer.
# sudo passwd -d \`whoami`

#### Now go to https://go.dev/dl/ to find the latest linux version of golang.
#### You want to find the version that ends with .linux-amd64.tar.gz.
#### At the time of writing the latest version is go1.20.2.linux-amd64.tar.gz.
#### Copy this version name "go1.20.2.linux-amd64.tar.gz", or the latest version available when you check.
#### Change the following two lines of code to account for the latest verison, then enter them into your cmder server connection:
# wget -c https://golang.org/dl/go1.20.2.linux-amd64.tar.gz
# sudo tar -C /usr/local -xvzf go1.20.2.linux-amd64.tar.gz
# sudo chown -R mainuser:mainuser /usr/local/go

# mkdir -p ~/go_projects/{bin,src,pkg}
# cd ~/go_projects
# ls
#### Uou should now see the 3 folders you just created :).
# nano ~/.profile
#### Add the following following lines to the nano file without the "#" on each line (you can paste them above the line that says #if running bash):


```python
#export GOPATH=$HOME/go
#export PATH=$PATH:$GOPATH/bin
#export PATH=$PATH:$GOPATH/bin:/usr/local/go/bin
#export PATH=$PATH:/usr/local/go/bin
#export GOPATH="$HOME/go_projects"
#export GOBIN="$GOPATH/bin"
```

#### Press okay to continue the multiline paste. Then remove all the #'s from the paste.
#### Press Ctrl + x, press y, press enter.

# . ~/.profile

# echo $PATH

# go version
#### Should print out the version of go you downloaded if you have completed the above steps successfully, if not then check the steps above again

#### Now you need to link your github account with your server via SSH so you can download avalanchego from github, follow the steps below

# cd /home/mainuser/.ssh
# ssh-keygen -t ed25519 -C "your_github_email@example.com"
#### (find your github email at the following link: https://github.com/settings/emails, also remove the " from the code line above)
#### Press enter to accept default file save location.
#### Enter passphrase if desired.
#### The next line of code will show the new ssh keys.
# ls -al ~/.ssh
# nano ~/.ssh/id_ed25519.pub
#### Copy the ENTIRE contents of the .pub file to windows clipboard, do this simply by highlighting the text and it will auto copy.
#### Press Ctrl + x, you should not need to press y or enter as you have not modified the file.
#### Now go to https://github.com/settings/keys
#### Press new ssh key and then paste the .pub file contents into github, set a appropriate title e.g "AVAX Server AX41-NVME" and click save.

# cd $GOPATH

# mkdir -p src/github.com/ava-labs
# cd src/github.com/ava-labs
# git clone git@github.com:ava-labs/avalanchego.git 
#### Accept the key fingerprint and continue connection with "yes".
#### Enter your passphrase for your SSH key if you created one.
# cd avalanchego
# sudo apt-get install build-essential
#### Press Y, then press enter twice when prompted.
# ./scripts/build.sh

# wget -nd -m https://raw.githubusercontent.com/ava-labs/avalanche-docs/master/scripts/avalanchego-installer.sh;\
# chmod 755 avalanchego-installer.sh;\
# ./avalanchego-installer.sh
#### Press 2 for cloud provider.
#### Press n if it doesn't show your IP (it didn't for me).
#### Enter your ip that you use to connect to your server.
#### Type "private" then type "on" to turn on state sync (unless you NEED the historical data, TIP: most don't).

# sudo systemctl status avalanchego
#### The above line should show active (running), if it does then just press q and exit the server and give it a day to bootstrap, come back in 24 hours or so to check in on the progress :)

# sudo journalctl -u avalanchego -f
#### This line enables to to read the system output and see when bootstrapping is nearly finished.
#### Press ctrl+C when you wish to stop reading node output.


```python

```

# Next day tasks: (Very important!)

#### Check that bopotstrapping is complete with the following health check.


```python
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health
```

#### If bootstrapping is complete then one of the final outputs of the health check should be: "healthy":true
#### Next you will NEED to backup your staking keys!!! (Very important!)

#### Open a terminal window on your Windows PC by typing CMD into the windows searchbar and clicking on command prompt.
#### MODIFY the following command to suit your circumstance before entering it into your terminal window:
# scp -r mainuser@xxx.xxx.xxx.xxx:/home/mainuser/.avalanchego/staking C:/Users/"WindowsUsername"/avalanche_backup_todays_date
#### Make sure to replace mainuser with whatever you called your login (if you followed my guide then you chose mainuser anyway so no need to change it), also make sure to replace xxx.xxx.xxx.xxx with your servers IP, also make sure to replace "todays_date" with the actual date...
#### Verify that your staker keys have been saved to the chosen directory on your windows pc and I also strongly recommend saving them to a pen stick or removable storage device for extra safety. If your node is ever shutdown or goes wrong and you need to restore your Node-ID on another server then you will need these files to restore the Node-ID and retain your staking uptime. Avalanche requires a MINIMUM staking uptime of 80%+ (at time of writing) for rewards payout. 

#### Next you'll need to find out your Node-ID. Run the command below from your linux server terminal cmder


```python
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeID"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```

#### Your public Node-ID looks something like this NodeID-6rRhirfsvBGvtxprBzEVR2AAVy99r9mJt. This ID is sharable to your friends if they want to delegate AVAX tokens to your node. You'll need this Node-ID in order to stake your AVAX.

# IMPORTANT!
# How to update and monitor your node.


```python
Firstly, you can always do a health check:
    
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health
```

#### Every so often a new version of AvalancheGo will come out. 
#### Be sure to follow the Avalanche Announcement Channel in Discord: https://discord.com/channels/578992315641626624/757973465444778084 
#### Or if you prefer Telegram: https://t.me/avalanche_announcements 
#### Or if you prefer Twitter: https://twitter.com/avalancheavax , https://twitter.com/_patrickogrady 

#### You should also use the official Avalanche Node notification service to monitor your node:
#### https://notify.avax.network/
#### You should also use the validator dashboard: (enter your NodeID into the NodeID box on the page)
#### https://stats.avax.network/dashboard/validator-health-check/
#### You should also use AllNodes:
#### https://check.allnodes.com
#### You should also use VScout
#### https://vscout.io/validator
#### You should also use AVASCAN:
#### https://avascan.info/staking/validator

# Now you are a pro at node monitoring (I suggest you bookmark all those pages above, with your NodeID entered to each, so you can quickly and easily check your node performance), how do you update your node when new versions come out?

#### Simply run:
# ./avalanchego-installer.sh
#### If the above doesn't work then:
# wget -nd -m https://raw.githubusercontent.com/ava-labs/avalanche-docs/master/scripts/avalanchego-installer.sh;\
# chmod 755 avalanchego-installer.sh;\
# ./avalanchego-installer.sh
#### Press 2 for cloud provider.
#### Press n if it doesn't show your IP (it didn't for me).
#### Enter your ip that you use to connect to your server.
#### Type "private" then type "on" to turn on state sync (unless you NEED the historical data, TIP: most don't).
#### Then run:
# ./avalanchego-installer.sh --reinstall
#### Press 2 for cloud provider.
#### Press n if it doesn't show your IP (it didn't for me).
#### Enter your ip that you use to connect to your server.
#### Type "private" then type "on" to turn on state sync (unless you NEED the historical data, TIP: most don't).
# GIVE THIS A FEW MINS FIRST, and then do a health check.


```python
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health
```

# You can also check uptime:


```python
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.uptime"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```

#### Now go back to the node monitoring sites above (AllNodes/VScout are usually the fastest to update in my experience) and check your node has updated to the latest revision


```python

```

# You are now finished with this guide. If you need any further help I reccomend going to the Avalanche discord (please be careful of scammers pretending to be from the Avalanche team that are reaching out to you in your Direct/Private messages)
#### Relevant Discord Channels below:
#### https://discord.com/channels/578992315641626624/620633143002660874
#### https://discord.com/channels/578992315641626624/757576823570825316


```python

```

# How to restore your Node-ID from backed up staking keys:

#### Lets imagine your node has gone offline and you are now trying to restore your staker keys to a fresh node, then the process you would follow is as follows: (bear in mind you can only have your staker keys active on 1 node at a time. If you have them on multiple nodes then I have no idea what will happen but I don't think it's good)
#### Open up a windows terminal by typing cmd into the windows search and clicking to open command prompt
#### Type the following 3 lines of code into your terminal, make sure to replace {your username}, {backup date}, mainuser and xxx.xxx.xxx.xxx with the actual values that reflect your circumstances.
# scp C:/Users/{your username}/avalanche_backup_{backup date}/staking/staker.crt mainuser@xxx.xxx.xxx.xxx:/home/mainuser/temp
# scp C:/Users/{your username}/avalanche_backup_{backup date}/staking/staker.key mainuser@xxx.xxx.xxx.xxx:/home/mainuser/temp
# scp C:/Users/{your username}/avalanche_backup_{backup date}/staking/signer.key mainuser@xxx.xxx.xxx.xxx:/home/mainuser/temp
#### Now you have copied our staking keys onto a temporary file in your server you'll need to move them to the staking directory from within the server. Open up cmder and log into your server then type:
# mv /home/mainuser/temp/staker.crt /home/mainuser/.avalanchego/staking/
# mv /home/mainuser/temp/staker.key /home/mainuser/.avalanchego/staking/
# mv /home/mainuser/temp/signer.key /home/mainuser/.avalanchego/staking/
#### You will have to overwrite those files to get your NodeID back then you can delete the temp folder:
# rm -r /home/mainuser/temp

#### after restoring staker keys do the following:

# sudo systemctl stop avalanchego
# sudo systemctl start avalanchego

#### check system running

# sudo systemctl status avalanchego
#### make sure port 9651 is opon, if you have UFW installed (you will if you followed this guide) then type.
# sudo ufw status numbered
#### You should see that 9651 is open along with the SSH port, if it isn't then do:
# sudo ufw allow 9651
# sudo ufw allow ssh
# sudo ufw disable
# sudo ufw enable
# sudo ufw status numbered
#### You should now see that both the SSH port and 9651 are open. 


```python

```
