# Debain 12(Bookwarm) - Installing NVIDIA GeForce RTX™ 3070 Display Driver 

Step by Step Instructions to Install Nvidia  GeForce RTX™ 3070 Display Driver for Lenovo Legion 5 Pro running Debain 12 Bookwarm .


1. UEFI Settings

    On system start , press F2 to enter into UEFI , select Discrete Graphics 

    Save and Exit

2. Start in non gui mode

    Power On 

    Press 'e' on GRUB menu 

    Edit the line that starts with linux and add 3 at the end of the line 

    Press F10 key  to save changes and reboot system in console mode.

3. Clean/Uninstall existing Nvidia drivers if any

    
    `sudo apt remove nvidia-driver`

    `sudo dpkg -l | grep nvidia`

    If you find any nvidia packages still with above command , then run

    `sudo apt remove nvidia-*`

    Remove any existing Nvidia dkms kernal modules 

    `ls -l /lib/modules/$(uname -r)/updates/dkms`

    if you see any *.ko files then remove them 

    `rm -rf /lib/modules/$(uname -r)/updates/dkms/*.ko`

    Reboot into console mode - see Step 2

4. Generate Module signing keys 

    Since we are using secure boot, we need to sign the nvidia driver moduels with our own trusted keys 

    4.1. Check  to see if secure boot is enabled

      `sudo mokutil --sb-state`

      if above command returns "Secureboot enabled" then your system is secure boot is enabled , otherwise enable secure boot from UEFI.

    4.2. Generate Private and Public keys

    ```
    mkdir ~/nvidia-certs
    cd ~/nvidia-certs
    # generate keys
    openssl req -new -x509 -newkey rsa:2048 -keyout nvidia.priv -outform DER -out nvidia.der -days 36500 -subj "/CN=Nvidia Display Driver/"
    # convert pub key  to pem format 
    openssl x509 -inform der -in nvidia.der -out nvidia.pem
    ```

5. Enroll Public Key in UEFI MOK ( Machine Owner Key)

    `
    cd ~/nvidia-certs
    `

    NOTE: Below command ask for onetime password ,  you must rember this password  , this is needed at the time of UEFI MOK key registration

    `
    sudo mokutil --import  nvidia.der
    `

    Check UEFI will prompt the key to register at next reboot 

    `sudo mokutil --list-new`

    if it prints the certificate information, then it means that we can register the mok key in UEFI while next reboot.

    Reboot  machine 

    It opens UEFI, and prompts for MOK key resirtation, it will inform  new mok key is found ,  do you want to enroll the key?

    Select enroll key

    Enter  password -  this is same password from step 5

    Boot into console mode 

    Validate that Key is registred in MOK

    `sudo dmesg | grep  'cert'`

    you should see the  message that  Nvidia related key  is registreded.

    You can also check by using below command

    `sudo mokutil --list-enrolled | grep 'Subject'`

    you should see 'Nvidia Display Driver' 

6. Install Nvidia Display Driver using apt

    edit  /etc/apt/sources.list
    
    `sudo vim /etc/apt/sources.list`

    and  add/edit deb http://deb.debian.org/debian/ bookworm line  to look like  below 

    `deb http://deb.debian.org/debian/ bookworm main contrib non-free non-free-firmware`

    upgrade apt

    `sudo apt upgrade` 

    install  nvidia drivers 

    `sudo apt install nvidia-driver firmware-misc-nonfree`
    
    above command will install nvidia drivers and kernal modules 

7. Sign  Nvidia Kernal modules 

    installed  kernal module files exists in /lib/modules/$(uname -r)/updates/dkms directory.

      
    `ls -l  /lib/modules/$(uname -r)/updates/dkms`

    You should see *.ko files. 
     
    Sign *.ko modules 

    ```
    VERSION="$(uname -r)"
    SHORT_VERSION="$(uname -r | cut -d . -f 1-2)"
    MODULES_DIR=/lib/modules/$VERSION
    KBUILD_DIR=/usr/lib/linux-kbuild-$SHORT_VERSION
    ```
    store  mok password into  shell  variable 

    ```
    # below command reads password into KBUILD_SIGN_PIN var , so enter the password after running the below command

    read -s KBUILD_SIGN_PIN
    export KBUILD_SIGN_PIN
    ```
    cd  to   /lib/modules/$(uname -r)/updates/dkms/

    `cd /lib/modules/$(uname -r)/updates/dkms/`

    run below script

    ```
    for i in *.ko ; do sudo --preserve-env=KBUILD_SIGN_PIN  "$KBUILD_DIR"/scripts/sign-file sha256 /home/anjan/nvidia/certs/nvidia.priv /home/anjan/nvidia/certs/nvidia.der "$i" ; done
    ```
8. When new Linux Kernal Version is released 

    if you updated your system to new kernal version , then follow Step 7  to sign the *.ko nvidia driver modules.
     