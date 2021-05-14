+++

Title = "chocolatey"
weight = 1

+++

#### Choco

Chocolatey is a package manager for windows and is quite useful in setting up windows quickly.

1. Ensure Administrative Shell Access

2. `Get-ExecutionPolicy` of `powershell` should not be set to Restricted.

   We can set it to `Bypass` to install all packages or `AllSigned` to get packages with quite bit of security

   Execute the following text

   ````power
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
   ````

3. Wait for some time for process to complete

4. Type `choco` or `choco -?` to check the installation.

#### Usage :

- Upgrading the chocolatey

  ````power
  choco upgrade chocolatey
  ````

- Installing a package

  ````power
  choco install gimp
  ````

#### Downloading basic packages

##### Minimal

````power
choco install -y googlechrome firefox adobereader 7zip vlc notepadplusplus 
````

##### Developer (Recommended)

Basic :

````power
choco install -y googlechrome firefox hexchat foxitreader mpc-hc k-litecodecpackfull vlc qbittorrent 7zip windirstat teracopy libreoffice-fresh
````

Tools :

````power
choco install -y notepadplusplus sublimetext3 vscode typora git gimp github-desktop javaruntime jdk11 jdk8 python virtualbox nvm nodejs wget curl dotnet3.5 php openssh microsoft-windows-terminal cmdermini
````

