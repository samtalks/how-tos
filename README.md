# Some general instructionals and guides

## How to set up Wordpress on DigitalOcean 

Adapted from the following sources:

* [DO's one-click instructions (5/29/14)](https://www.digitalocean.com/community/tutorials/one-click-install-wordpress-on-ubuntu-14-04-with-digitalocean) 
* 




1. If you don't have an SSH key added to your digitalocean account, add it before creating your droplet. 
  * On terminal on a Mac `$ cd /.ssh` then `$ ls` to check your folder's contents.
  * If you don't have the files `id_rsa` and `id_rsa.pub` in this folder, create them by `$ ssh-keygen -t rsa -C "YOU@YOUR-DOMAIN.com"`. When prompted for passphrase, hit enter for to *not* set a password. You should then see your key fingerprint (looks like a long MAC Address) and your key's randomart image. 
  * Copy the public key from here `$ cat ~/.ssh/id_rsa.pub`. 
2. 
3. Create a new droplet by selecting the default size, a region, and select "Wordpress on Ubuntu 14.04" from the application tab. 
2. 
