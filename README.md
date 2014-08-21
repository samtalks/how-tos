# Some general instructionals and guides

## How to set up Wordpress on DigitalOcean 

Adapted from the following sources:

* [DO's one-click instructions (5/29/14)](https://www.digitalocean.com/community/tutorials/one-click-install-wordpress-on-ubuntu-14-04-with-digitalocean) 
* [PublishingWithWordpress Guide](http://publishingwithwordpress.com/installing-wordpress-digital-ocean/)


1. If you don't have an SSH key added to your digitalocean account, add it before creating your droplet. SSH is much more robust against brute force password attacks, so you want to use SSH over password authentication.
  * On terminal on a Mac `$ cd /.ssh` then `$ ls` to check your folder's contents.
  * If you don't have the files `id_rsa` and `id_rsa.pub` in this folder, create them by `$ ssh-keygen -t rsa -C "YOU@YOUR-DOMAIN.com"`. When prompted for passphrase, hit enter for to *not* set a password. You should then see your key fingerprint (looks like a long MAC Address) and your key's randomart image. 
  * Copy the public key from here `$ cat ~/.ssh/id_rsa.pub`. 
  * In DigitalOcean, go to "SSH Keys" in the left panel and "Add SSH Key".
  * Paste the key in and give your key a name (something like your first name and hostname). 
2. With your SSH added to DO, you can now setup your Droplet.
  * "CREATE" a new droplet.
  * Add your planned hostname (YOUR-DOMAIN.COM)
  * Select the default size, a region, and "Wordpress on Ubuntu 14.04" from the Application tab
  * In "Add optional SSH Keys" click on your Key so that it turns from gray to blue. 
  * Leave the "Settings" per defaults and "Create Droplet". Your root login password will be emailed to you.
  * Copy your IP Address from DigitalOcean to your clipboard
  * In your terminal do  `$ ssh root@[IP ADDRESS]`. If you get something like this:
    ```
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
    Someone could be eavesdropping on you right now (man-in-the-middle attack)!
    It is also possible that a host key has just been changed.
    The fingerprint for the RSA key sent by the remote host is
    [LOOKS LIKE LONG MAC ADDRESS].
    Please contact your system administrator.
    Add correct host key in /Users/[YOU]/.ssh/known_hosts to get rid of this message.
    Offending RSA key in /Users/[YOU]/.ssh/known_hosts:16
    RSA host key for [IP ADDRESS] has changed and you have requested strict checking.
    Host key verification failed.
    ```
    Delete the "offending" line from your `~/.ssh/known_hosts` file and then try again.
  * Now you should get something like this:
    ```
    The authenticity of host '[IP ADDRESS] ([IP ADDRESS])' can't be established.
    RSA key fingerprint is [LOOKS LIKE A LONG MAC ADDRESS].
    Are you sure you want to continue connecting (yes/no)? 
    ```
    Type `yes`.
  * You will be prompted for the password. Go to your email and copy the password that DigitalOcean sent you. Paste it into the terminal. Then change the password (and keep the records safely somewhere). 
  * Copy the wp-admin password that this pre-installation of Wordpress provides you and paste it into some notepad or somewhere. You will need this later.
3. At this point, DigitalOcean's one-click instructions has you install Wordpress. We're not going to do that yet. PublishingWithWordpress' guide has some optimization instructions we'll do first. Not sure if this order is necessary, but the author does them first so we'll follow suit. While still in your SSH terminal session, do the following in order: 
    ```
    #=> NOTE: The prompt on the server may show a hashtag # instead of dollar sign $ as on your machine. We'll keep using dollar sign notation.
    $ sudo apt-get update
    $ sudo apt-get upgrade
    $ sudo a2enmod rewrite
    $ dd if=/dev/zero of=/swapfile bs=1M count=1024
    $ mkswap /swapfile
    $ swapon /swapfile
    $ sudo nano /etc/fstab
    #=> ADD the following line to the fstab file:
    /swapfile swap swap defaults 0 0
    #=> verify the swap file is active (should show 1GB free):
    $ free
4. Now to install Wordpress on your server. Note: If you want to set up your wordpress install with the correct hostname (by pointing your nameservers to this IP address to set up your hostname), you can do so now. Otherwise, you can do that later and go directly to the IP address for now.
  * Go to your hostname (or IP address) on your browser and enter "admin" and the password you copied onto your notepad from earlier to log in. Fill in the form, create WP credentials, and installation should be successful.
  * Log in using your new credentials. Voila.
  
5. Security clean up of your users, passwords, etc.
  * 
