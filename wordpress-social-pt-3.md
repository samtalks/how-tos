# Connecting DNS nameservers

### If you are managing multiple snapshots of different apps on a single droplet...

Follow these directions, they are adapted from [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean)

1. In your domain registrar (e.g., Godaddy), go to the nameservers and add custom nameservers:
  * ns1.digitalocean.com
  * ns2.digitalocean.com
  * ns3.digitalocean.com
  This will now take a few hours to propagate. Now we have to do stuff on DigitalOcean's end. **WARNING: Doing this will disable web access from anywhere from a few hours to 48 hours.**
2. In DigitalOcean, click on the **DNS** button on the left nav panel.
3. Click on the big blug **ADD DOMAIN** button.
4. On the right dropdown, select your droplet. By doing so, the IP address in the center will automatically fill in. 
5. Fill in the domain in the left box and click **CREATE DOMAIN**.
