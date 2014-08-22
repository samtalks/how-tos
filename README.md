# Some general instructionals and guides

## How to set up Wordpress on DigitalOcean 

Adapted from the following sources:

* [DO's one-click instructions (5/29/14)](https://www.digitalocean.com/community/tutorials/one-click-install-wordpress-on-ubuntu-14-04-with-digitalocean) 
* [PublishingWithWordpress Guide](http://publishingwithwordpress.com/installing-wordpress-digital-ocean/)
* [DO's locking down root passwords after SSH access](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)

1. If you don't have an SSH key added to your digitalocean account, add it before creating your droplet. SSH is much more robust against brute force password attacks, so you want to use SSH over password authentication.
  * On terminal on a Mac `$ cd /.ssh` then `$ ls` to check your folder's contents.
  * If you don't have the files `id_rsa` and `id_rsa.pub` in this folder, create them by `$ ssh-keygen -t rsa -C "YOU@YOUR-DOMAIN.com"`. When prompted for passphrase, hit enter for to *not* set a password. You should then see your key fingerprint (looks like a long MAC Address) and your key's randomart image. 
  * Copy the public key from here `$ cat ~/.ssh/id_rsa.pub`. 
  * In DigitalOcean, go to "SSH Keys" in the left panel and "Add SSH Key".
  * Paste the public key in and give your key a name of the computer it's accessing from (something like "john-mba-2012-13"). 
2. With your SSH added to DO, you can now setup your Droplet.
  * "CREATE" a new droplet.
  * Add your planned hostname (YOUR-DOMAIN.COM)
  * Select the default size, a region, and "Wordpress on Ubuntu 14.04" from the Application tab
  * In "Add optional SSH Keys" click on your Key so that it turns from gray to blue. You will also no longer receive an email with the root login password.  
  * Leave the "Settings" per defaults and "Create Droplet". 
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
  * Copy the wp-admin password that this pre-installation of Wordpress provides you and paste it into some notepad or somewhere. You will need this later.
3. At this point, DigitalOcean's one-click instructions has you install Wordpress. We're not going to do that yet. PublishingWithWordpress' guide has some optimization instructions we'll do first. Not sure if this order is necessary, but the author does them first so we'll follow suit. While still in your SSH terminal session, do the following in order: 

    ```
    #=> NOTE: The prompt on the server may show a hashtag # instead of dollar sign $ as on your machine. We'll keep using dollar sign notation.
    $ sudo apt-get update
    $ sudo apt-get upgrade       #=> "dist-" deals with dependencies but adds ~270MB 
    $ sudo a2enmod rewrite
    $ dd if=/dev/zero of=/swapfile bs=1M count=1024
    $ mkswap /swapfile
    $ swapon /swapfile
    $ sudo nano /etc/fstab
    #=> ADD the following line to the fstab file:
    /swapfile swap swap defaults 0 0
    #=> verify the swap file is active (should show 1GB free):
    $ free
    ```

4. Now to install Wordpress on your server. Note: If you want to set up your wordpress install with the correct hostname (by pointing your nameservers to this IP address to set up your hostname), you can do so now. Otherwise, you can do that later and go directly to the IP address for now.
  * Go to your hostname (or IP address) on your browser and enter "admin" and the password you copied onto your notepad from earlier to log in. Fill in the form, create WP credentials, and installation should be successful.
  * Log in using your new credentials. Voila.
5. You also have access to SQL in your SSH session. To access SQL during your SSH session, you can get your password by: `$ cat /root/.my.cnf` 
6. Remove the "authentication.. restricted area" login window. 
  * `$ nano /etc/apache2/apache2.conf`
  * `CTRL` + ` W ` and search for "wp-admin".
  * Put a "#" in front of every line below:
    ```
    <DirectoryMatch ^.*/wp-admin/>
        AuthType Basic
        AuthName "Please login to your droplet via SSH for login details."
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
    </DirectoryMatch>
    ```
  * Save and exit the file. 
  * `$ service apache2 restart`  to restart the server. 

#### Security clean up of logins and passwords.
Once you're finished, at some point you will want to make your site more secure by eliminating passwords insecurely stored on your server, disabling password logins, etc. 

If you need to add SSH keys on a droplet already created, use [these instructions](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets). 


#### Personal Favorite plugins

* [bbpress](https://wordpress.org/plugins/bbpress/)
* [Simple custom CSS](https://wordpress.org/plugins/simple-custom-css/screenshots/) (Make sure to backup your css frequently)
* [Mailchimp for wordpress](http://wordpress.org/plugins/mailchimp-for-wp/) (Don't mistake with other mailchimp plugins)
