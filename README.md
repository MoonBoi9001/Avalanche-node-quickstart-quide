# Avalanche-node-quickstart-quide
This guide is intended to help Windows users get their Avalanche node (Linux based server) setup for validation purposes.

Consider donating some AVAX (feel free to send me literally anything) if this guide is useful 0x721644dff35504c8F9A3c389d7C4dCE5D8afC4d2 thank you so much. 

Disclaimer:
This guide is provided for educational and informational purposes only. Neither the author nor any associated parties can be held responsible for any losses, damages, or issues that may arise from following the instructions provided. By using this guide, you acknowledge that you are participating in the Avalanche network as a validator entirely at your own risk.
Setting up a cryptocurrency staking validator on the Avalanche network involves multiple steps, and the outcome can vary depending on your specific hardware, software, and network configurations. The author cannot guarantee that following this guide will result in a fully functional validator or ensure that you will receive staking rewards.
Before proceeding, please make sure you have a thorough understanding of the Avalanche network and the risks associated with cryptocurrency staking. Furthermore, it is essential to stay up-to-date with any changes to the Avalanche network or staking requirements, as these may affect your validator's performance and rewards eligibility.
As a validator, you must understand that maintaining an up-to-date node is critical for continued participation in the Avalanche network and earning staking rewards. You are responsible for keeping your node running with the latest required version of AvalancheGo and ensuring that all necessary packages are updated as needed.
The Avalanche network may introduce new features, security patches, or performance improvements that require an updated version of the software. If you fail to update your node and its dependencies, your validator may become ineligible for staking rewards or face penalties. It is your responsibility to monitor the official Avalanche communication channels and stay informed about any changes or updates that may impact your node's performance and staking eligibility.
Terms of Service Compliance: By following this guide, you agree to comply with any applicable terms of service, rules, or regulations associated with the Avalanche network and any relevant third-party services or platforms. The author and any associated parties take no responsibility for issues, losses, or damages that may arise due to violations of any terms of service. It is your responsibility to familiarize yourself with and adhere to these terms while participating in the Avalanche network as a validator.
By following this guide, you agree to assume full responsibility for keeping your node updated and acknowledge that neither the author nor any associated parties will be held responsible for any issues that may arise. You also release the author and any associated parties from any liability. If you are uncertain about any steps or require further assistance, please consult the official Avalanche documentation or seek help from the community.
 
Prerequisites
A reliable internet connection.
A minimum of 2,000 AVAX tokens for staking. (Accurate at time of writing, may be subject to change)
Basic understanding of command-line interfaces.
1. Download & install the latest full version of CMDER (a Linux terminal emulator for Windows that allows you to log into your server): https://cmder.app/
2. Create an account on GitHub: https://github.com/signup?source=login
3. Generate a new SSH keypair to log securely into your server with, if you do not already have a SSH keypair on your PC.
To check if you already have one, go to the following directory on your windows pc
C:/Users/{enter your username here}/.ssh
If you see a file that says id_rsa and a seperate file that says id_rsa.pub then you alread have a keypair that you can use.
If you don't have any files or the .ssh directory doesn't exist then you will have to make a keypair, follow the steps below:
Create a suitable folder e.g "hetzner_germany_avax_server_AX41-NVME" in this directoy C:/Users/{enter your username here}/.ssh for saving your ssh keys inside.
Open the newly created folder.
Launch command prompt by typing cmd into windows search and pressing enter, this will launch a terminal window.
Type "ssh-keygen" into the terminal window.
Enter a file to save your ssh keypair e.g C:/Users/{enter your username here}/.ssh/{enter your folder name here}/id_rsa
Enter a SECURE key passphrase which will keep your SSH key encrypted on your windows device, you NEED to remember this!
Confirm that the newly created keypair is saved into the new folder you created previously.
Copy the file that says id_rsa (not the one that says id_rsa.pub).
Go to the following directory on your windows pc:
C:/Users/{enter your username here}/.ssh
Paste a copy of your file id_rsa into this directory.
Go to the following directory on your windows pc.
C:/Users/{enter your username here}/.ssh/{enter your folder name here}
Open the file that says id_rsa.pub in notepad and then minimise notepad, you will come back to it later, you can close the file directoy explorer now.
 
Guide starts here if you have completed the prerequsites
Start by purchasing AX41-NVME on Hetzner in Germany or Finland, be cognizant of 1-off setup fees
https://www.hetzner.com/dedicated-rootserver/ax41-nvme/configurator#/
Keep the option Primary IPv4 checked and selected, you will need it
Select latest version of Ubuntu LTS, in my case, at the time of writing, this is Ubuntu 22.04.1 LTS (base)
Upgrade to ECC RAM near the bottom of the order page if you want (not needed)
Click order, which will take you to the checkout page, on the checkout page you will see you can specify to login to your server with password or public key. You will use public key. If you followed the earlier part of my guide then your notepad is open already and you can copy your public key. Paste your public key into the key data field and then save and complete your order. If you are not sure where to find your public key then go to the following directory C:/Users/{enter your username here}/.ssh and find the file that says id_rsa.pub.
You will have to wait up to a few hours untill the server is available.
 
When configured you will get an email, you can check the configuration at: https://robot.hetzner.com/server
Copy your new AX41-NVME servers IP, if it doesn't show up, it's probably not configured yet, this can take a few hours.
Launch cmder then type:
ssh root@{ip}
Make sure to replace "{ip}" with the AX41-NVME ip address you copied from the link above.
Type yes to trust your new servers key fingerprint.
When prompted to enter passphrase for your ssh keypair enter the passphrase you setup earlier when you generated the ssh keypair.
 
Congrats you are now logged into your new server and ready to beging configuring it to validate the Avalanche network
Type the following into your server 1 by 1:
sudo apt update && sudo apt upgrade
The above line updates the package list and the installed packages to the latest versions.
Y (to continue) and may also need to press okay a few times during the configuration process, you don't need to restart any more processes than recommended, just press enter.
sudo adduser mainuser
The above line create a new user account on the server called "mainuser".
Set a new password, doesn't really matter what you set it to as you won't use it, just make it sufficiently complex so its not hacked before you disable password login (make sure to rememer it though!!!).
Press enter a few times to set default new user values.
Press Y to show information is correct.
usermod -aG sudo mainuser
The above line adds the user you created "mainuser" to the list of accounts that can do sudo commands.
Log out of your server by closing cmder.
Relaunch cmder.
ssh mainuser@{ip}
Press enter to skip ssh passphrase.
Enter your newly created main user password (the one you were supposed to remember).
If you take too long then it may say "Connection closed by '{ip}' port 22", just try again but do it faster this time.
mkdir -p ~/.ssh
Open the id_rsa.pub file in your ssh directoy C:/Users/{enter your username here}/.ssh using notepad and then copy the ENTIRE contents of the file.
Once the public key string is copied go back to cmder where you are logged into your new hetzner server.
echo {public_key_string} >> ~/.ssh/authorized_keys
Make sure to replace "{public_key_string}" with your ENTIRE ssh public key string that you coped from notepad.
Accept the warning message that the long text may make the console non-responsive.
Verify the above line worked with the following where you will see your key:
nano /home/mainuser/.ssh/authorized_keys
If you can see your SSH key then you completed the step successfully, close nano text editor by pressing CTRL + X.
chmod -R go= ~/.ssh
chown -R mainuser:mainuser ~/.ssh
sudo nano /etc/ssh/sshd_config
Enter your created main user password (the one you were supposed to remember).
Find and change the following lines: (remember to remove the # at the start of the line)
PermitRootLogin yes
PubkeyAuthentication yes
PasswordAuthentication no
PermitEmptyPasswords yes
Then press Ctrl + x, then press Y to save modified buffer, then write the file to the existing file by simply pressing enter.
sudo systemctl restart ssh
sudo apt install ufw
sudo ufw allow ssh
sudo ufw allow 9651
sudo ufw enable
Press y to proceed.
sudo ufw status numbered
You should see that ssh is allowed in from anywhere on port 22. And the Port 9651 is open and accessible.
sudo visudo
Enter the following line:
mainuser ALL=(ALL) NOPASSWD:ALL
I think you can enter the above line anywhere, but i put it under the "# user privilege specification" section.
Save the file with Crtl + x, then press Y, then enter to save the modified buffer.
sudo passwd -d `whoami`
Now go to https://go.dev/dl/ to find the latest linux version of golang.
You want to find the version that ends with .linux-amd64.tar.gz.
At the time of writing the latest version is go1.20.2.linux-amd64.tar.gz.
Copy this version name "go1.20.2.linux-amd64.tar.gz", or the latest version available when you check.
Change the following two lines of code to account for the latest verison, then enter them into your cmder server connection:
wget -c https://golang.org/dl/go1.20.2.linux-amd64.tar.gz
sudo tar -C /usr/local -xvzf go1.20.2.linux-amd64.tar.gz
sudo chown -R mainuser:mainuser /usr/local/go
mkdir -p ~/go_projects/{bin,src,pkg}
cd ~/go_projects
ls
Uou should now see the 3 folders you just created :).
nano ~/.profile
Add the following following lines to the nano file without the "#" on each line (you can paste them above the line that says #if running bash):
#export GOPATH=$HOME/go
#export PATH=$PATH:$GOPATH/bin
#export PATH=$PATH:$GOPATH/bin:/usr/local/go/bin
#export PATH=$PATH:/usr/local/go/bin
#export GOPATH="$HOME/go_projects"
#export GOBIN="$GOPATH/bin"
Press okay to continue the multiline paste. Then remove all the #'s from the paste.
Press Ctrl + x, press y, press enter.
. ~/.profile
echo $PATH
go version
Should print out the version of go you downloaded if you have completed the above steps successfully, if not then check the steps above again
Now you need to link your github account with your server via SSH so you can download avalanchego from github, follow the steps below
cd /home/mainuser/.ssh
ssh-keygen -t ed25519 -C "your_github_email@example.com"
(find your github email at the following link: https://github.com/settings/emails, also remove the " from the code line above)
Press enter to accept default file save location.
Enter passphrase if desired.
The next line of code will show the new ssh keys.
ls -al ~/.ssh
nano ~/.ssh/id_ed25519.pub
Copy the ENTIRE contents of the .pub file to windows clipboard, do this simply by highlighting the text and it will auto copy.
Press Ctrl + x, you should not need to press y or enter as you have not modified the file.
Now go to https://github.com/settings/keys
Press new ssh key and then paste the .pub file contents into github, set a appropriate title e.g "AVAX Server AX41-NVME" and click save.
cd $GOPATH
mkdir -p src/github.com/ava-labs
cd src/github.com/ava-labs
git clone git@github.com:ava-labs/avalanchego.git
Accept the key fingerprint and continue connection with "yes".
Enter your passphrase for your SSH key if you created one.
cd avalanchego
sudo apt-get install build-essential
Press Y, then press enter twice when prompted.
./scripts/build.sh
wget -nd -m https://raw.githubusercontent.com/ava-labs/avalanche-docs/master/scripts/avalanchego-installer.sh;%5C
chmod 755 avalanchego-installer.sh;\
./avalanchego-installer.sh
Press 2 for cloud provider.
Press n if it doesn't show your IP (it didn't for me).
Enter your ip that you use to connect to your server.
Type "private" then type "on" to turn on state sync (unless you NEED the historical data, TIP: most don't).
sudo systemctl status avalanchego
The above line should show active (running), if it does then just press q and exit the server and give it a day to bootstrap, come back in 24 hours or so to check in on the progress :)
sudo journalctl -u avalanchego -f
This line enables to to read the system output and see when bootstrapping is nearly finished.
Press ctrl+C when you wish to stop reading node output.
 
Next day tasks: (Very important!)
Check that bopotstrapping is complete with the following health check.
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health
If bootstrapping is complete then one of the final outputs of the health check should be: "healthy":true
Next you will NEED to backup your staking keys!!! (Very important!)
Open a terminal window on your Windows PC by typing CMD into the windows searchbar and clicking on command prompt.
MODIFY the following command to suit your circumstance before entering it into your terminal window:
scp -r mainuser@xxx.xxx.xxx.xxx:/home/mainuser/.avalanchego/staking C:/Users/"WindowsUsername"/avalanche_backup_todays_date
Make sure to replace mainuser with whatever you called your login (if you followed my guide then you chose mainuser anyway so no need to change it), also make sure to replace xxx.xxx.xxx.xxx with your servers IP, also make sure to replace "todays_date" with the actual date...
Verify that your staker keys have been saved to the chosen directory on your windows pc and I also strongly recommend saving them to a pen stick or removable storage device for extra safety. If your node is ever shutdown or goes wrong and you need to restore your Node-ID on another server then you will need these files to restore the Node-ID and retain your staking uptime. Avalanche requires a MINIMUM staking uptime of 80%+ (at time of writing) for rewards payout.
Next you'll need to find out your Node-ID. Run the command below from your linux server terminal cmder
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeID"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
Your public Node-ID looks something like this NodeID-6rRhirfsvBGvtxprBzEVR2AAVy99r9mJt. This ID is sharable to your friends if they want to delegate AVAX tokens to your node. You'll need this Node-ID in order to stake your AVAX.
IMPORTANT!
How to update and monitor your node.
Firstly, you can always do a health check:
    
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health
Every so often a new version of AvalancheGo will come out.
Be sure to follow the Avalanche Announcement Channel in Discord: https://discord.com/channels/578992315641626624/757973465444778084
Or if you prefer Telegram: https://t.me/avalanche_announcements
Or if you prefer Twitter: https://twitter.com/avalancheavax , https://twitter.com/_patrickogrady
You should also use the official Avalanche Node notification service to monitor your node:
https://notify.avax.network/
You should also use the validator dashboard: (enter your NodeID into the NodeID box on the page)
https://stats.avax.network/dashboard/validator-health-check/
You should also use AllNodes:
https://check.allnodes.com
You should also use VScout
https://vscout.io/validator
You should also use AVASCAN:
https://avascan.info/staking/validator
Now you are a pro at node monitoring (I suggest you bookmark all those pages above, with your NodeID entered to each, so you can quickly and easily check your node performance), how do you update your node when new versions come out?
Simply run:
./avalanchego-installer.sh
If the above doesn't work then:
wget -nd -m https://raw.githubusercontent.com/ava-labs/avalanche-docs/master/scripts/avalanchego-installer.sh;%5C
chmod 755 avalanchego-installer.sh;\
./avalanchego-installer.sh
Press 2 for cloud provider.
Press n if it doesn't show your IP (it didn't for me).
Enter your ip that you use to connect to your server.
Type "private" then type "on" to turn on state sync (unless you NEED the historical data, TIP: most don't).
Then run:
./avalanchego-installer.sh --reinstall
Press 2 for cloud provider.
Press n if it doesn't show your IP (it didn't for me).
Enter your ip that you use to connect to your server.
Type "private" then type "on" to turn on state sync (unless you NEED the historical data, TIP: most don't).
GIVE THIS A FEW MINS FIRST, and then do a health check.
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"health.health"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health
You can also check uptime:
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.uptime"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
Now go back to the node monitoring sites above (AllNodes/VScout are usually the fastest to update in my experience) and check your node has updated to the latest revision
 
You are now finished with this guide. If you need any further help I reccomend going to the Avalanche discord (please be careful of scammers pretending to be from the Avalanche team that are reaching out to you in your Direct/Private messages)
Relevant Discord Channels below:
https://discord.com/channels/578992315641626624/620633143002660874
https://discord.com/channels/578992315641626624/757576823570825316
 
How to restore your Node-ID from backed up staking keys:
Lets imagine your node has gone offline and you are now trying to restore your staker keys to a fresh node, then the process you would follow is as follows: (bear in mind you can only have your staker keys active on 1 node at a time. If you have them on multiple nodes then I have no idea what will happen but I don't think it's good)
Open up a windows terminal by typing cmd into the windows search and clicking to open command prompt
Type the following 3 lines of code into your terminal, make sure to replace {your username}, {backup date}, mainuser and xxx.xxx.xxx.xxx with the actual values that reflect your circumstances.
scp C:/Users/{your username}/avalanche_backup_{backup date}/staking/staker.crt mainuser@xxx.xxx.xxx.xxx:/home/mainuser/temp
scp C:/Users/{your username}/avalanche_backup_{backup date}/staking/staker.key mainuser@xxx.xxx.xxx.xxx:/home/mainuser/temp
scp C:/Users/{your username}/avalanche_backup_{backup date}/staking/signer.key mainuser@xxx.xxx.xxx.xxx:/home/mainuser/temp
Now you have copied our staking keys onto a temporary file in your server you'll need to move them to the staking directory from within the server. Open up cmder and log into your server then type:
mv /home/mainuser/temp/staker.crt /home/mainuser/.avalanchego/staking/
mv /home/mainuser/temp/staker.key /home/mainuser/.avalanchego/staking/
mv /home/mainuser/temp/signer.key /home/mainuser/.avalanchego/staking/
You will have to overwrite those files to get your NodeID back then you can delete the temp folder:
rm -r /home/mainuser/temp
after restoring staker keys do the following:
sudo systemctl stop avalanchego
sudo systemctl start avalanchego
check system running
sudo systemctl status avalanchego
make sure port 9651 is opon, if you have UFW installed (you will if you followed this guide) then type.
sudo ufw status numbered
You should see that 9651 is open along with the SSH port, if it isn't then do:
sudo ufw allow 9651
sudo ufw allow ssh
sudo ufw disable
sudo ufw enable
sudo ufw status numbered
You should now see that both the SSH port and 9651 are open.
