## Disclaimer:

Please refer to the [README.md](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide#disclaimer) file for the full disclaimer related to this guide.

## Prerequisites

- A reliable internet connection.
- A minimum of 2,000 AVAX tokens for staking (accurate at the time of writing; subject to change).
- Basic understanding of command-line interfaces.

#### 1. Download and Install CMDER

Download and install the latest full version of CMDER, a Linux terminal emulator for Windows, which enables you to log into your server: https://cmder.app/  

#### 2. Create a GitHub Account

Create an account on GitHub if you don't already have one: https://github.com/signup?source=login

#### 3. Generate an SSH Keypair

Generate a new SSH keypair to securely log into your server. If you already have a keypair on your PC, you can skip this step.

- #### 3.1 Check for Existing Keypair

    Go to the following directory on your Windows PC:
    
    `C:/Users/{your_username}/.ssh`
    
    If you see files named `id_rsa` and `id_rsa.pub`, you already have a keypair that you can use.

- #### 3.2 Create a New SSH Keypair

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

# Guide starts here if you have completed the prerequisites 

## Registering your server

#### 1. Purchase AX41-NVME on Hetzner

Start by purchasing an AX41-NVME server on Hetzner in Germany or Finland, keeping in mind any one-time setup fees: [Hetzner AX41-NVME Configurator](https://www.hetzner.com/dedicated-rootserver/ax41-nvme/configurator#/)
- Keep the option `Primary IPv4` checked and selected, as you will need it.
- Select the latest version of Ubuntu LTS (at the time of writing, this is Ubuntu 22.04.1 LTS (base)).
- Optionally consider upgrading to ECC RAM near the bottom of the order page (not required).

#### 2. Complete Your Order

Click "Order" to proceed to the checkout page. On the checkout page, you will have the option to log in to your server using either a password or a public key. Choose the public key option.
- If you followed the earlier part of this guide, your Notepad with the public key should be open already. Copy your public key and paste it into the "Key Data" field.
- If you're not sure where to find your public key, go to the following directory: `C:/Users/{your_username}/.ssh` and find the file named `id_rsa.pub`.

Save and complete your order. You may have to wait up to a few hours until the server is available.

## Logging into your server

Once your server has finished being configured, it should appear in the `Server ID` section of the [Hetzner Robot dashboard](https://robot.hetzner.com/server). For example, you might see `AX41-NVMe #1871660`.

1. Find your server's IP address
    Click on your Server's ID and then copy the IP address. You can make a note of the IP somewhere for future reference, or you can always find it using the link provided above.

2. Connect to your server using Cmder

    Make sure to replace "`{IP}`" with the IP address of your server that you copied from the link above.
    
    Launch: `Cmder.exe`
    
    Run: `ssh root@{IP}` 

3. Trust the key fingerprint

    Your Linux terminal emulator will now connect to your server and prompt you to trust the key fingerprint. This will only be asked once, during the initial setup. Type `yes` and press Enter.

4. Enter your secure passphrase for your SSH keypair when prompted.

## Beginning configuration of your server

Now that you have successfully logged into your server, you're ready to begin configuration. The following steps will guide you through setting up your node. Make sure to follow the steps sequentially, as entering the code out of order may result in a failure to set up the node. It's recommended that you read the explanation of the code before entering it into your server so you are aware of what each line performs.

### Step 1: Update the package list and installed packages

Run: `sudo apt update && sudo apt upgrade`

After entering, press `Y` (to continue) and may also need to press `Enter` on your keyboard to select okay a few times during the configuration process. You don't need to restart any more processes than recommended; simply press `Enter`.

### Step 2: Create a non-root user account

Run: `sudo adduser mainuser`

After entering, set a new password. It doesn't really matter what you set it to, as you won't use it. Just make it sufficiently complex so it's not hacked before you disable password login (but make sure to remember it). Press `Enter` a few times to set default new user values. Then press `Y` to show information is correct.

### Step 3: Add the non-root user to the sudo group

Run: `usermod -aG sudo mainuser`

### Step 4: Log out of your server and log back in as the new user

Run: `exit`

This will log you out of your server. Close Cmder, then relaunch it and enter the following command, replacing `{ip}` with your server's IP address:

Run: `ssh mainuser@{ip}`

Press `Enter` to skip the SSH passphrase, this time only instead you will use your newly created user password (the one you were supposed to remember). If you take too long, it may say "Connection closed by '{ip}' port 22"; just try again, but do it faster this time.

### Step 5: Configure SSH key-based authentication for the new user

1. First make a new SSH directory in your server:

    Run: `mkdir -p ~/.ssh`
    
2. Then, from your Windows File Explorer, open the `id_rsa.pub` file in your SSH directory (`C:/Users/{enter your username here}/.ssh`) using Notepad, and then copy the entire contents of the file. Once the public key string is copied, go back to Cmder where you are logged into your server. Type the following, replacing `{public_key_string}` with your entire SSH public key string that you copied from Notepad:

    Run: `echo '{public_key_string}' >> ~/.ssh/authorized_keys`
    
3. Verify the above line worked with the following command, which will show your public SSH key:

    Run: `nano /home/mainuser/.ssh/authorized_keys`
    
   If you can see your public SSH key then you have completed the step successfully. Close the Nano text editor by pressing `CTRL` + `X`.

### Step 6: Secure the SSH configuration

1. Secure your SSH configuration by setting the appropriate permissions, ownership, and updating the configuration file:

    Run: `chmod -R go= ~/.ssh`
    
    Run: `chown -R mainuser:mainuser ~/.ssh`
    
2. Open the SSH configuration file with nano:

    Run: `sudo nano /etc/ssh/sshd_config`
    
   Then enter your user password (the one you were supposed to remember).

3. Find and change the following lines:

    - Change `PermitRootLogin yes` to `PermitRootLogin no`
    - Make sure `PubkeyAuthentication` is set to `yes`
    - Change `PasswordAuthentication yes` to `PasswordAuthentication no`
    - Change `PermitEmptyPasswords yes` to `PermitEmptyPasswords no`
    - Change `Port 22`to 'Port 2222'

4. Save and exit the file:
    - Press `Ctrl + X` to initiate the save process.
    - Press `Y` to confirm the save.
    - Press `Enter` to save the modified buffer and exit.
    
5. Restart the SSH service to apply the changes:
    
    Run: `sudo systemctl restart ssh`
    
    Your SSH configuration has been made more secure by following these steps. However, it's crucial to remember that no system can be completely secure, and you should always stay up to date with best practices and security recommendations.

### Step 7: Set up the firewall

1. For this step, we will be using UFW because it's incredibly simple to use and very powerful. Install UFW by running the following command:

    Run: `sudo apt install ufw`

2. Allow port 2222 for SSH. If your IP is fixed, or you use a VPN, you can change this to `sudo ufw allow 2222 from {IP}`, which is more secure and limits the attack surface of a DDOS attack. It is strongly advised, although technically not strictly necessary. If your fixed IP changes in the future, you will have to update the firewall from your server dashboard, as you won't be able to login via SSH from an IP that has not been whitelisted.

    Run: `sudo ufw allow 2222`

3. Allow port 9651 for your Avalanche node to communicate with other nodes on the Avalanche network. Do not change this.

    Run: `sudo ufw allow 9651`

4. Enable UFW and confirm when prompted:

    Run: `sudo ufw enable`

   Press `y` to proceed.

5. Check the UFW status to ensure that ports 2222 and 9651 are open and accessible. Port 22 should not be open. If port 22 is open, you can close it with `sudo ufw delete allow 22`.

    Run: `sudo ufw status numbered`
   
### Step 8: How to log into your server after Steps 6 & 7

1. Please note that from now on, to log into your server, you will have to specify port 2222. Use the following command when logging in with Cmder:

    Run: `ssh mainuser@{IP} -p 2222`
    
    Enter your secure passphrase for your SSH keypair when prompted.

### Step 9: Configure sudo privileges

1. Open the sudoers file for editing by running the following command:

    Run: `sudo visudo`
    
2. Add the following line to grant your user sudo privileges without a password:

    Add: `mainuser ALL=(ALL) NOPASSWD:ALL`

3. Save and exit the file:
    - Press `Ctrl + X` to initiate the save process.
    - Press `Y` to confirm the save.
    - Press `Enter` to save the modified buffer and exit.

4. Disable the password requirement for your current user by running the following command:

    Run: `sudo passwd -d $(whoami)`

## Download Go onto your configured server

Now you have completed the configuration process, you can download and install Go, which you will need for your node to validate the Avalanche network.

### Step 1: Download & Install the latest Go version

1. Go to https://go.dev/dl/ to find the latest Linux version of Golang. You want to find the version that ends with `.linux-amd64.tar.gz`. At the time of writing, the latest version is `go1.20.2.linux-amd64.tar.gz`. Copy the version name `go1.20.2.linux-amd64.tar.gz`, or the latest version available when you check. Then change the first two lines of code below to account for the latest version and enter them into your Cmder terminal:

    Run: `wget -c https://golang.org/dl/go1.20.2.linux-amd64.tar.gz`
    
    Run: `sudo tar -C /usr/local -xvzf go1.20.2.linux-amd64.tar.gz`
    
    The first command does this thingy, while the second command does this other thingy.

2. Take ownership of the `/usr/local/go` directory

    Run: `sudo chown -R mainuser:mainuser /usr/local/go`

3. Make 3 new folders inside the `/usr/local/go` directory

    Run: `mkdir -p ~/go_projects/{bin,src,pkg}`

### Step 2: Set up your environment variables

1. Use a text editor such as nano to open the `.profile` file so you can configure environment variables.

    Run: `nano ~/.profile`

2. Add the following following lines to the file (you can paste them above the line that says #if running bash):

        export GOPATH=$HOME/go
        export PATH=$PATH:$GOPATH/bin
        export PATH=$PATH:/usr/local/go/bin
        export GOPATH="$HOME/go_projects"
        export GOBIN="$GOPATH/bin"

3. Save and exit the file:
    - Press `Ctrl + X` to initiate the save process.
    - Press `Y` to confirm the save.
    - Press `Enter` to save the modified buffer and exit.

4. Reload the `.profile` file to apply the changes:

    Run: `. ~/.profile`

5. Verify that the environment variables are set correctly:

    This command displays the content of the `PATH` environment variable, ensuring that the Go-related paths have been added successfully.
    
    Run: `echo $PATH`

6. Verify the installed Go version:

    This command prints the version of Go installed on your system. If the output shows the expected Go version, it indicates that the environment variables have been configured correctly. If not, double-check the previous steps.
    
    Run: `go version`

## Link your Github account with your server via SSH

Now you need to link your GitHub account with your server via SSH so you can download AvalancheGo from GitHub.

### Step 1: Open your SSH directory and generate an SSH key pair

Run: `cd /home/mainuser/.ssh`

Run: `ssh-keygen -t ed25519 -C your_github_email@example.com`

Make sure to replace `your_github_email@example.com` with your actual GitHub account email. You can find your GitHub email at the following link: [GitHub](https://github.com/settings/emails)

- Press `Enter` to accept the default file save location.
- Enter a secure passphrase if desired.

### Step 2: Copy your SSH public key

The next section of code will show the new SSH keys.

Run: `ls -al ~/.ssh`

Run: `nano ~/.ssh/id_ed25519.pub`

Copy the ENTIRE contents of the .pub file to the Windows clipboard, do this simply by highlighting the text and it will auto copy.

Press `Ctrl + X`, you should not need to press Y or Enter as you have not modified the file.

### Step 3: Paste your SSH key into GitHub

Now go to the [GitHub SSH Key Page](https://github.com/settings/keys)

Press `New SSH key` and then paste the .pub file contents into GitHub, set an appropriate title e.g., `AVAX Server AX41-NVME` and click `Save`.

## Clone and build the AvalancheGo Github

### Step 1: Clone AvalancheGo Github

1. Change directory to your GOPATH.

    Run: `cd $GOPATH`
    
2. Make a directory for ava-labs.

    Run: `mkdir -p src/github.com/ava-labs`
    
3. Change directory to newly created ava-labs directory.
    
    Run: `cd src/github.com/ava-labs`
    
4. Clone the Ava-Labs Github Repo to this directory.
    
    Run: `git clone git@github.com:ava-labs/avalanchego.git `

    Accept the key fingerprint and continue connection with `yes`. Then enter your passphrase for your SSH key if you created one.
    
5. Change directory to newly created ava-labs directory.

    Run: `cd avalanchego`

### Step 2: Build the AvalancheGo Github
    
1. Install & build the directory.
    
    Run: `sudo apt-get install build-essential`
    
    Press `Y`, then press `Enter` twice when prompted.
    
    Run: `./scripts/build.sh`

2. Run the installer script.

        wget -nd -m https://raw.githubusercontent.com/ava-labs/avalanche-docs/master/scripts/avalanchego-installer.sh;\
        chmod 755 avalanchego-installer.sh;\
        ./avalanchego-installer.sh
    
    Press `2` for cloud provider.

    Press `n` if it doesn't show your IP (it didn't for me).

    Enter your IP that you use to connect to your server.

    Type `private` then type `on` to turn on state sync (unless you NEED the historical data, TIP: most don't).

3. Check everything is working as intended.

    Run: `sudo systemctl status avalanchego`

    The above line should show `active (running)`, if it does then just press `q` and then exit the server, give it a day or so to finish bootstrapping.

    - Note you can also monitor the system output with the following: Run `sudo journalctl -u avalanchego -f`. Press `Ctrl + c` when you wish to stop reading node output.

## Backup your staking keys & find your NodeID:

### Step 1: Check that bootstrapping is complete.

1.  You can check that bootstrapping is complete with the following health check.

    ```
    curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health"
    }' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health
    ```

    If bootstrapping is complete then one of the final outputs of the health check should be: `"healthy":true`.

### Step 2: Backup your staking keys.

1. Next you will NEED to backup your staking keys!!! (Very important!)

2. Open a terminal window on your Windows PC by typing CMD into the windows searchbar and clicking on command prompt.

3. MODIFY the following command to suit your circumstances before entering it into your terminal window:

    Run: `scp -r mainuser@xxx.xxx.xxx.xxx:/home/mainuser/.avalanchego/staking C:/Users/"WindowsUsername"/avalanche_backup_todays_date`

    Make sure to replace `mainuser` with whatever you called your login (if you followed my guide then you chose mainuser anyway so no need to change it), also make sure to replace `xxx.xxx.xxx.xxx` with your servers IP, also make sure to replace `avalanche_backup_todays_date` with the actual date e.g `avalanche_backup_21_03_2023`...

4. Verify that your staker keys have been saved to the chosen directory on your Windows PC and I also strongly recommend saving them to a pen stick or removable storage device for extra safety. If your node is ever shutdown or goes wrong and you need to restore your NodeID on another server then you will need these files to restore the NodeID and retain your staking uptime. Avalanche requires a MINIMUM staking uptime of 80%+ (at time of writing) for rewards payout. 

### Step 3: Find your NodeID:

1. Next you'll need to find out your NodeID. Run the command below from your linux server terminal cmder

    ```
    curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeID"
    }' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
    ```
    
    Your public Node-ID looks something like this `NodeID-6rRhirfsvBGvtxprBzEVR2AAVy99r9mJt`. This ID is sharable to your friends if they want to delegate AVAX tokens to your node. You'll need this NodeID in order to stake your AVAX.

## How to update, monitor and restore your node.

#### Note: This is an incredibly important part of this guide. You will need to monitor your node for the duration of time you are staking. Failing to monitor your node is likely to lead to a rewards payout miss for you and your delegators. At the time of writing the required uptime for staking rewards is 80% and there is no slashing. Non-default subnets may have different requirements to the C/X/P chains. It's your responsibility to be aware of the full requirements to recieve staking rewards and your responsibility to research the full risks involved for validating any subnets. 

### Step 1: How to monitor your node. 

1. Firstly, you can do a health check when logged into your server by entering the following into the terminal:
    
    ```
    curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health"
    }' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health
    ```
    
    - Note that this check is not sufficient on it's own to ensure your node is functioning appropriately!

2. Every so often a new version of AvalancheGo will come out. Be sure to follow the public Avalanche Announcements on Discord, Telegram and Twitter:

    - https://discord.com/channels/578992315641626624/757973465444778084 
    - https://t.me/avalanche_announcements 
    - https://twitter.com/avalancheavax , https://twitter.com/_patrickogrady 

3. You can (and should!) also check directly from the avalanchego releases page of the ava-labs github. This may be the best way to get the most accurate and timely information. 

    - https://github.com/ava-labs/avalanchego/releases
    
    On the release page linked above, click the drop down arrow next to `Watch` at the top right hand side of the page then select `Custom`, then select `Releases` and click `Apply`. This should send you notifications whenever there is a new release. You can also get Android/iOS push notifications for releases if you download the Github app onto your device.

4. You should also use the official Avalanche Node notification service to monitor your node:

    - https://notify.avax.network/
    
5. You should also use the validator dashboard: (enter your NodeID into the NodeID box on the page)

    - https://stats.avax.network/dashboard/validator-health-check/
    
6. You should also use AllNodes:

    - https://check.allnodes.com
    
7. You should also use VScout

    - https://vscout.io/validator

8. You should also use AVASCAN:

    - https://avascan.info/staking/validator

9. I suggest you bookmark the links in steps 4 to 7 inclusive, with your NodeID entered to each, so you can quickly and easily check your node performance. It is not sufficient to only use 1 provider.  


### Step 2: How to update your node when new versions come out

1. If you have the installer script configured (you should if you followed this guide) then you can run:

   `./avalanchego-installer.sh`
    
    If the above doesn't work then try:
    
    ```
    wget -nd -m https://raw.githubusercontent.com/ava-labs/avalanche-docs/master/scripts/avalanchego-installer.sh;\
    chmod 755 avalanchego-installer.sh;\
    ./avalanchego-installer.sh --reinstall
    ```
        
    Press `2` for cloud provider.

    Press `n` if it doesn't show your IP (it didn't for me).

    Enter your IP that you use to connect to your server.

    Type `private` then type `on` to turn on state sync (unless you NEED the historical data, TIP: most don't).
    
2. Give the prior step a few mins first (suggested 3-5 mins) and then do a health check to confirm everything is working.

    ```
    curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health"
    }' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health
    ```

    You can also check uptime:
    
    ```
    curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.uptime"
    }' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
    ```
    
    Now go back to the [node monitoring sites above](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#step-1-how-to-monitor-your-node) (AllNodes/VScout are usually the fastest to update in my experience) and check your node has updated to the latest revision.

### Step 3: How to restore your NodeID from backed up staking keys:

Lets imagine your node has gone offline and you are now trying to restore your staking keys to a fresh node, then the process you would follow is as follows: (bear in mind you should only have your staking keys active on 1 node at a time to prevent potential network conflicts).

1. Open up a windows terminal by typing `cmd` into the windows search and clicking to open command prompt.

2. Type the following 3 lines of code into your terminal, make sure to replace `{your username}`, `{backup date}`, `mainuser` and `xxx.xxx.xxx.xxx` with the actual values that reflect your circumstances. (you may have to edit the full directory path if your staking keys are saved elsewhere. This will use SSH to copy and paste your staking keys from the directory where they are saved to a temporary directory on your server.

    Run: `scp C:/Users/{your username}/avalanche_backup_{backup date}/staking/staker.crt mainuser@xxx.xxx.xxx.xxx:/home/mainuser/temp`
    
    Run: `scp C:/Users/{your username}/avalanche_backup_{backup date}/staking/staker.key mainuser@xxx.xxx.xxx.xxx:/home/mainuser/temp`
    
    Run: `scp C:/Users/{your username}/avalanche_backup_{backup date}/staking/signer.key mainuser@xxx.xxx.xxx.xxx:/home/mainuser/temp`

3. Now you have copied your staking keys into a temporary directory in your server, you'll need to move them to the staking directory. Open up Cmder and log into your server then type:

    Run: `mv /home/mainuser/temp/staker.crt /home/mainuser/.avalanchego/staking/`
    
    Run: `mv /home/mainuser/temp/staker.key /home/mainuser/.avalanchego/staking/`
    
    Run: `mv /home/mainuser/temp/signer.key /home/mainuser/.avalanchego/staking/`

    Note: You will have to overwrite those files to get your prior NodeID back.

4. Delete the temp folder:

    Run: `rm -r /home/mainuser/temp`

5. Your Staking keys should now be restored onto your server. Now you'll need to restart the Avalanchego process:

    Run: `sudo systemctl stop avalanchego`
    
    Run: `sudo systemctl start avalanchego`

6. Check the system process is running:

    Run: `sudo systemctl status avalanchego`
    
7. Make sure port 9651 is open, if you have UFW installed (you will if you followed this guide) then type.

    Run: `sudo ufw status numbered`
    
8. You should see that 9651 is open along with the SSH port (we are using port 2222 in this guide), if it isn't then do:

    Run: `sudo ufw allow 9651`
    
    Run: `sudo ufw allow 2222`
    
    Run: `sudo ufw disable`
    
    Run: `sudo ufw enable`
    
    Run: `sudo ufw status numbered`
    
    You should now see that both the SSH port (2222) and 9651 are open.


# You are almost finished with this guide. If you need any further help I reccomend going to the Avalanche discord. 

#### Please be careful of scammers reacing out to you in your Direct/Private messages, they may pretend to be from the Avalanche team.

#### Relevant Discord Channels below:
#### https://discord.com/channels/578992315641626624/620633143002660874
#### https://discord.com/channels/578992315641626624/757576823570825316

