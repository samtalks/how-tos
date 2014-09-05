# Connecting DNS nameservers

### Setting up domain name for URL

Follow these directions, they are adapted from [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean). The comments are also very helpful.

1. In your domain registrar (e.g., Godaddy), go to the nameservers and add custom nameservers:
  * ns1.digitalocean.com
  * ns2.digitalocean.com
  * ns3.digitalocean.com
  This will now take a few hours to propagate. Now we have to do stuff on DigitalOcean's end. **WARNING: Doing this will disable web access from anywhere from a few hours to 48 hours.**
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

### Restoring from a snapshot

You can restore from snapshots. If you do, keep in mind that doing this


