# Avalanche Node Quickstart Guide for Windows Users

This guide is intended to help Windows users set up their Avalanche node on a Linux-based server for validation purposes. The guide covers topics ranging from prerequisites to configuring your validator on the Avalanche network, saving and restoring your staking keys (important!), and monitoring/updating your node.

## Table of Contents

1. [Disclaimer](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide#disclaimer)
2. [Prerequisites](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#prerequisites)
3. [Registering your server](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#registering-your-server)
4. [Logging into your server](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#logging-into-your-server)
5. [Beginning configuration of your server](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#beginning-configuration-of-your-server)
6. [Download Go onto your configured server.](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#download-go-onto-your-configured-server)
7. [Link your Github account with your server via SSH](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#link-your-github-account-with-your-server-via-ssh)
8. [Clone and build the AvalancheGo Github](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#clone-and-build-the-avalanchego-github)
9. [Backup your staking keys & Find your NodeID](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#backup-your-staking-keys--find-your-nodeid)
10. [How to update, monitor and restore your node](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide/blob/main/Guide%20for%20Windows%20PC.md#how-to-update-monitor-and-restore-your-node)
10. [Support and Donations](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide#support-and-donations)
11. [License](https://github.com/MoonBoi9001/Avalanche-node-quickstart-quide#license)

## Getting Started

To get started, open the file `Guide for Windows PC.md` for the full walkthrough. Enjoy!

## Support and Donations

If you find this guide useful, please consider donating some AVAX to the following address: `0x721644dff35504c8F9A3c389d7C4dCE5D8afC4d2`. Any amount is greatly appreciated!

## License

This project is licensed under the [MIT License](LICENSE).

## Disclaimer

#### Educational and Informational Purposes Only
This guide is provided for educational and informational purposes only, on an "as-is" basis, without any warranties or guarantees, either expressed or implied, regarding its accuracy, completeness, or effectiveness. The author does not claim to be an expert in the subject matter and has created this guide based on personal experience, research, and understanding. Users are encouraged to verify the information independently and consult additional sources.

#### No Liability
Neither the author nor any associated parties can be held responsible for any losses, damages, or issues that may arise from following the instructions provided. By using this guide, you acknowledge that you are participating in the Avalanche network as a validator entirely at your own risk.

#### No Guaranteed Outcome
Setting up a cryptocurrency staking validator on the Avalanche network involves multiple steps, and the outcome can vary depending on your specific hardware, software, and network configurations. The author cannot guarantee that following this guide will result in a fully functional validator or ensure that you will receive staking rewards.

Before proceeding, please make sure you have a thorough understanding of the Avalanche network and the risks associated with cryptocurrency staking. Furthermore, it is essential to stay up-to-date with any changes to the Avalanche network or staking requirements, as these may affect your validator's performance and rewards eligibility.

#### Responsibility for Updates
As a validator, you must understand that maintaining an up-to-date node is critical for continued participation in the Avalanche network and earning staking rewards. You are responsible for keeping your node running with the latest required version of AvalancheGo and ensuring that all necessary packages are updated as needed.

The Avalanche network may introduce new features, security patches, or performance improvements that require an updated version of the software. If you fail to update your node and its dependencies, your validator may become ineligible for staking rewards or face penalties. It is your responsibility to monitor the official Avalanche communication channels and stay informed about any changes or updates that may impact your node's performance and staking eligibility.

#### Terms of Service Compliance
By following this guide, you agree to comply with any applicable terms of service, rules, or regulations associated with the Avalanche network and any relevant third-party services or platforms. The author and any associated parties take no responsibility for issues, losses, or damages that may arise due to violations of any terms of service. It is your responsibility to familiarize yourself with and adhere to these terms while participating in the Avalanche network as a validator.

By following this guide, you agree to assume full responsibility for keeping your node updated and acknowledge that neither the author nor any associated parties will be held responsible for any issues that may arise. You also release the author and any associated parties from any liability. If you are uncertain about any steps or require further assistance, please consult the official Avalanche documentation, seek help from the community, or consult a professional.

The author will not be held liable for any errors, omissions, or inaccuracies in the guide or for any decisions made based on the information provided in the guide. Users are encouraged to seek professional advice or consult the official documentation and resources related to the subject matter, as the guide may not cover all aspects or may not be suitable for all users.
