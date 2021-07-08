# Setting_Up_WSL
A repository showing how to setup WSL (mostly focused on WSL 2), set up a WSL distribution, and move an existing WSL distribution.

## Setting up WSL:

### Step 1 - Enable the Windows Subsystem for Linux
Open PowerShell as Administrator and run:
`dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
We recommend now moving on to step #2, updating to WSL 2, but if you wish to only install WSL 1, you can now restart your machine and move on to Step 6 - Install your Linux distribution of choice.

### Step 2 - Check requirements for running WSL 2
To update to WSL 2, you must be running Windows 10.
* For x64 systems: Version 1903 or higher, with Build 18362 or higher.
* For ARM64 systems: Version 2004 or higher, with Build 19041 or higher.
* Builds lower than 18362 do not support WSL 2. 

If you are running Windows 10 version 1903 or 1909, open "Settings" from your Windows menu, navigate to "Update & Security" and select "Check for Updates". Your Build number must be 18362.1049+ or 18363.1049+, with the minor build # over .1049.

### Step 3 - Enable Virtual Machine feature
Before installing WSL 2, you must enable the Virtual Machine Platform optional feature. Your machine will require virtualization capabilities to use this feature.
Open PowerShell as Administrator and run:
`dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`

### Step 4 - Download the Linux kernel update package
If you're not sure what kind of machine you have, open Command Prompt or PowerShell and enter: `systeminfo | find "System Type"`.
On non-English Windows versions, you might have to modify the search text, for example, in German it would be `systeminfo | find "Systemtyp"`.

For x64 machines: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
For ARM64 machines: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi

Run the update package downloaded in the previous step. (Double-click to run - you will be prompted for elevated permissions, select ‘yes’ to approve this installation.)

Once the installation is complete, move on to the next step - setting WSL 2 as your default version when installing new Linux distributions. (Skip this step if you want your new Linux installs to be set to WSL 1).

### Step 5 - Set WSL 2 as your default version
Open PowerShell and run this command to set WSL 2 as the default version when installing a new Linux distribution:
`wsl --set-default-version 2`

### Step 6 - Install your Linux distribution of choice
Open the Microsoft Store and search your preferred Linux distribution. Select it. Press `Get` and then `Install`.

WSL 2 with a Linux distribution should be setup on your device now.

## Moving WSL:

Normally WSL would be stored in the C drive, however it can be moved [On Windows 10, needs to be version 1903 or above. (Tested only with WSL 2)].
This method enables the user to run in a way like before, however the Linux distribution icon will be removed. To run it you would have to run `wsl.exe` in the terminal, or search for it in the search bar and run it from there rather than clicking on the Linux distribution icon.

Moving using the WSL Command Line Tool (within a Windows Terminal):

1. Export the distribution. Create a .tar file with the distribution to move using wsl.exe --export
`wsl.exe --export <DistributionName> <Tar-FileName>`
For instance, to export an Ubuntu distribution, you can use
`C:\> wsl.exe --export Ubuntu c:\data\ubuntu.tar`

2. Import the distribution into the target folder. Then, you can import the exported distribution into another folder
`wsl.exe --import <DistributionName> <Folder-To-Install> <Tar-FileName>`
For instance, to import the exported Ubuntu into a new UbuntuCustom distribution, you can use
`C:\> wsl.exe --import UbuntuCustom d:\wsl\UbuntuCustom c:\data\ubuntu.tar`

After the importing is done you can uninstall the Linux distribution you have from within the Apps page in Windows Settings.

3. Set default user of distribution installed via wsl --import
Open PowerShell and run:
`Get-ItemProperty Registry::HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss\*\ DistributionName | Where-Object -Property DistributionName -eq <DistributionName> | Set-ItemProperty -Name DefaultUid -Value <Uid>`
Where `<DistributionName>` is the name of the distribution you imported and `<Uid>` is the user id. You can check user id by running `wsl.exe` and then on it run `cat /etc/passwd | grep`.
For instance, for an imported UbuntuCustom distribution, and having checked your user id is 1000, you can use
`Get-ItemProperty Registry::HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss\*\ DistributionName | Where-Object -Property DistributionName -eq UbuntuCustom | Set-ItemProperty -Name DefaultUid -Value 1000`

### References:

https://docs.microsoft.com/en-us/windows/release-health/release-information
https://docs.microsoft.com/en-us/windows/wsl/install-win10
https://stackoverflow.com/questions/38779801/move-wsl-bash-on-windows-root-filesystem-to-another-hard-drive
https://github.com/microsoft/WSL/issues/3974

### Versions applicable:

Windows 10, needs to be version 1903 or above.
Tested on version 20H2 (OS Build 19042).