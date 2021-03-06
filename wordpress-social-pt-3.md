# Setup DNS nameservers, MX records

### 1. Setting up domain name for URL

Follow these directions, they are adapted from [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean). The comments are also very helpful.

1. In your domain registrar (e.g., Godaddy), go to the nameservers and add custom nameservers:
  * ns1.digitalocean.com
  * ns2.digitalocean.com
  * ns3.digitalocean.com

  This will now take a few hours to propagate. Now we have to do stuff on DigitalOcean's end. 
  
  **WARNING:** Doing so will disable web access from anywhere from a few hours to 48 hours.
2. In DigitalOcean, click on the **DNS** button on the left nav panel.
3. Click on the big blug **ADD DOMAIN** button.
4. On the right dropdown, select your droplet. By doing so, the IP address in the center will automatically fill in. 
5. Fill in the domain in the left box and click **CREATE DOMAIN**.
6. From here, add a CNAME that redirects **www** to **@**. Don't create a catchall `*` that redirects to **@**.

Wait a few hours for the DNS registries to update the information. Once it does, typing in the domain name in the browser address bar will directly take you to where you want to go. **BUT!,** browsing around in the website will still show the ip address as the domain in the URL. To fix this, make changes in the **Settings** panel in your Wordpress admin section:

1. Change **WordPress Address (URL)** to the proper domain name *without* a trailing `/`.
2. Change **Site Address (URL)** to the proper domain name *without* a trailing `/`.
You must do the above together - doing only the first will not allow you to log in. 

If you make the above mistake, refer to [this wordpress doc](http://codex.wordpress.org/Changing_The_Site_URL) to fix the solution from within the database. Fixing it from wp-config.php file is only a temporary solution.


### 2. Restoring from a snapshot

You can restore from snapshots. If you do, keep in mind that doing this will cause at least 15 minutes of downtime, depending on the size of your droplet and snapshots.

1. First, make sure you've created snapshots of everything you need.
2. Go to your active droplet and Destroy it. Do so without Scrubbing the data (this is faster. Scrubbing is for extra security). Once you start this process, it will take at least 15 minutes or so while the website starts to go down.
3. Once the droplet is completely gone, create a new one using a snapshot. This part is much faster. 
4. Once this is done, update your ssh by going to your terminal  `cd ~/.shh` and then `subl known_hosts`. Remove the previous host ssh key.
5. Go to `ssh root@YOUR_DOMAIN` and follow directions to create a new known host key.


### 3. Setting up MX records

Once you transfer nameservers to DigitalOcean, DigitalOcean takes over the management of all records including MX records. If you've changed nameservers from Godaddy, you might get this confused because Godaddy's management interface still allows you to make changes to those records. However, changes you make in Godaddy will *not* propagate. 

So, let's set up MX records. Here are the tools you will need to check what your MX records are displayed as. 

* Check MX records here: [MXtoolbox](http://mxtoolbox.com/)
* Or, better yet, if you use Google Apps for email, use [Google's toolbox](https://toolbox.googleapps.com/apps/checkmx/check?from=support.google.com&origin=checkmx-widget&domain=coalitionedu.org). They provide detailed instructions on how to correct errors (some of which we'll add here).

1. When you're ready, set up MX records in DigitalOcean. If using Google Apps, simple click the Google MX shortcut button. However, as of this writing, DigitalOcean's fill in of the MX records are slightly off from Google's documentation. (Basically, any occurrence of "googlemail" in DigitalOcean's tool should be replaced with "google"). 

2. Set up an SPF record by following [These DigitalOcean instructions](https://www.digitalocean.com/community/tutorials/how-to-create-a-spf-record-for-your-domain-with-google-apps)

3. It's recommended you set up DKIM and DMARC as well, but setting those up is a very involved process. There may be a easier solution in the future. 





