+++

Title = "WSL"
weight = 3

+++

### Installation : WSL 2

##### Step : Enabling WSL Feature 

Execute following command in `powershell` to enable WSL in windows.

````powe
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
````

This sets up WSL 1.

##### Step : Enabling WSL 2 (requires build > 18362)

Requires Virtualization Features

````power
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
````

[Install Kernel Update Package](https://aka.ms/wsl2kernel) : Download Package available at this link!

Setting up WSL 2 as default version

````power
wsl --set-default-version 2
````

##### Step : Installing Linux Distro

Simple go to Microsoft store and download distro of your choice.

Debian is recommended.

##### (Optional) : Installing Windows Terminal

Windows Terminal is a exciting product from Microsoft as it allows multiple tabs and better Terminal for `powershell` than the one that is shipped by default.

```bash
choco install microsoft-windows-terminal
```

##### (Optional) : Setup up the WSL using Unix Setup
