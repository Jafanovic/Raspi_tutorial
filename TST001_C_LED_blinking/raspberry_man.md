![malina](https://upload.wikimedia.org/wikipedia/commons/b/b1/Raspbian_logo.png)

Užitečné linky: 
> - [Markdown VSCode man](https://www.markdownguide.org/tools/vscode/)

# HOW TO - Raspberry Pi

## How to check the Raspbian version on my Raspberry Pi

1. You can check the release of Raspbian, simply reading the content of the os-release file.

    ``` cat /etc/os-release ```

    ```
        PRETTY_NAME="Raspbian GNU/Linux 10 (buster)"
        NAME="Raspbian GNU/Linux"
        VERSION_ID="10"
        VERSION="10 (buster)"
        VERSION_CODENAME=buster
        ID=raspbian
        ID_LIKE=debian
        HOME_URL="http://www.raspbian.org/"
        SUPPORT_URL="http://www.raspbian.org/RaspbianForums"
        BUG_REPORT_URL="http://www.raspbian.org/RaspbianBugs"
    ```

2. if you want to know the Kernel version in your Raspberry Pi, you can use the uname command.

    > ``` uname -a ```

    ```
        Linux raspberrypi 5.10.63-v7+ #1496 SMP Wed Dec 1 15:58:11 GMT 2021 armv7l GNU/Linux
    ```

## How to update and upgrade my Raspbian

1. First, you need to check if enough free space is available on yout SD card.

    > ``` df -h ```

2. If enough space is available, then you have to update the package manager apt.

    > ``` sudo apt-get update ```

3. Once apt is updated, you can upgrade your Raspbian distribution.

    > ``` sudo apt-get dist-upgrade ```
    
    > ``` sudo apt-get full-upgrade ```

_Note: 29.01.2022 jsem to zkoušel a zdá se že upgrade na verzi 11 se nekoná. Po nakouknutí na  [raspberry.com](https://www.raspberrypi.com/software/operating-systems/) jsem zjistil že releasedate je "January 28th 2022" no tak uvidíme_

## Raspberry Pi FTP Server Setup Guide

1. **Step: Update System Packages**

2. **Step: Install FTP Server - vsftpd**
   
    _There are several utilities available for setting up an FTP server on Raspberry Pi. In this tutorial, we will use the open-source **vsftpd** utility._

    > ``` sudo apt install vsftpd ```

3. **Step: Edit Configuration File**
   
   > ```sudo nano /etc/vsftpd.conf```

  * Find (CTRL + W) and uncomment the following lines by removing the hash (#) sign:

    > ```write_enable=YES```

    > ```local_umask=022 ```

    > ``` chroot_local_user=YES ```

    > ``` anonymous_enable=YES ```   note: bezpečnější je NO 

  * Press CTRL + X and confirm with Y to save the settings and for exit press ENTER.
  
4. **Step: Create FTP Directory**
   
   Create an FTP directory to use for transferring files. A subdirectory is needed since the root directory cannot have write permissions.
   
   > ``` mkdir -p /home/[user]/FTP/[subdirectory_name]```

    Replace [user] with the relevant user. Replace [subdirectory_name] with a name of your choice. The default user on Raspberry Pi OS is "pi."

    Poznámka **nenastavil** jsem ``` [subdirectory_name] ``` a opravdu to nefungovalo. Total Commander hlásil:

    _vsftpd on linux: 500 OOPS: vsftpd: refusing to run with writable root inside chroot()_

    To jsem podle [webu](https://ma.ttias.be/vsftpd-linux-500-oops-vsftpd-refusing-run-writable-root-inside-chroot/) spravil následovně:

    Do konfiguračního souboru vsftpd.conf (``` sudo nano /etc/vsftpd.conf```) jsem na konec souboru přidal 
    
    > ``` allow_writeable_chroot=YES  ```  

    a provedl restart znovu služby 


5. **Step: Restart Vsftpd Daemon**:
   
   > ``` sudo service vsftpd restart ```

   Now the FTP server is set up and running on the Raspberry Pi.

6. FTP Server Test: Total Commander setting: 

   Obtain the Pi's IP address by running the following command in the Raspberry Pi terminal: 

   > ``` ifconfig ``` 

   ![ifconfig](https://phoenixnap.com/kb/wp-content/uploads/2021/04/obtain-ip-address-using-ifconfig.png)

   Použij **CTRL-F** a vyplň:

   * Název relace: _pi ftypko_  
   * Hostitel: _192.168.1.102:21_  (ip adresa : port), Port number is 21 (default)
   * Jméno uživatele: _pi_
   * Heslo: _raspberry_


Now you know how to set up an FTP server on your Raspberry Pi. The solution is simple and works well when transferring files between two computers. You can use this setup as private cloud storage and keep the costs low.
