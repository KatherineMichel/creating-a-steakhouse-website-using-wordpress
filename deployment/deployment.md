# Deployment

## Primary Software

* Domain: [GoDaddy Domain Name Search](https://www.godaddy.com/domains/domain-name-search)
* Domain private registration: [GoDaddy Private Registration](https://www.godaddy.com/domainaddon/private-registration.aspx)
* Host: [Digital Ocean](https://www.digitalocean.com)
* Theme: [Sage from HighGrade Lab](http://www.highgradelab.com/sage)
* [SSH](https://en.wikipedia.org/wiki/Secure_Shell) client: [PuTTY](http://www.putty.org) and Mac terminal

## SEO

* Webmaster: [Google Webmaster Tools](https://www.google.com/webmasters/tools)
* General SEO: [Yoast SEO for WordPress](https://yoast.com/wordpress/plugins/seo) plugin
* XML Sitemap Generator: [Google XML Sitemap Generator](https://wordpress.org/plugins/google-sitemap-generator) plugin

## Utilities

* Photo shopping: [Gimp](https://www.gimp.org)
* Zipfile management [PeaZip](https://sourceforge.net/projects/peazip)
* Device simulation and testing: Google Canary Chrome Development Tools [Device Mode](https://developers.google.com/web/tools/chrome-devtools/iterate/device-mode/?hl=en)
* File sharing: [Google Docs](https://www.google.com/docs/about)
* Smart lossy compression: [TinyPNG](https://tinypng.com)

## Considered

* WordPress Hosting Dashboard for Digital Ocean: [ServerPilot](https://serverpilot.io)
* [All-in-One WP Migration](https://wordpress.org/plugins/all-in-one-wp-migration) tool as a backup migration plan

## Process

* Set up primary software accounts
* Set up [Digital Ocean One-Click WordPress Install](https://www.digitalocean.com/community/tutorials/how-to-use-the-wordpress-one-click-install-on-digitalocean)
* [Configured GoDaddy domain to point to Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars)
* [Configured domain in Digital Ocean control panel](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean)
* Enabled installation by [connecting to droplet with SSH](https://www.digitalocean.com/community/tutorials/how-to-connect-to-your-droplet-with-ssh) via PuTTY and Mac terminal, changing password
* Created special Apache server configuration via SSH and [Nano](https://www.nano-editor.org) to enable upload of larger premium theme
* Used local environment to test WordPress before implementing in host environment
* Used Google Canary Chrome Development Tools [Device Mode](https://developers.google.com/web/tools/chrome-devtools/iterate/device-mode/?hl=en) for device simulation for testing; also crowdsourced device testing
* Used [Yoast SEO for WordPress](https://yoast.com/wordpress/plugins/seo) plugin, [Google XML Sitemap Generator](https://wordpress.org/plugins/google-sitemap-generator) plugin, and [Google Webmaster Tools](https://www.google.com/webmasters/tools) plugin for search indexing
* Implemented XML-RPC attacks fix

## Digital Ocean Specs

    WordPress on 14.04
    $10/month $0.015/per hour
    1 GB/1 CPU
    30 GB SSD Disk
    2 TB Transfer
    Datacenter region: New York
    Backup

## Trouble Shooting

### Theme Installation

While attempting to install the theme, I received the following message. I would need to adjust Apache settings.

![](deployment-images/are-you-sure.jpg)

Apache server file access via SSH and Nano

    $ sudo nano /etc/php5/apache2/php.ini

Altered settings

    upload_max_filesize = 50M
    post_max_size = 50M
    memory_limit = 200M
    max_input_time = 150

Server restart

    $ service apache2 restart

I was then able to upload and activate the theme without incident. 

### Downtime

I was searching for the website daily in Google in anticipation that it would show up in the results. When it did, I clicked on the website link and the website displayed an error. 

![](deployment-images/error-establishing-a-database-connection.jpg)

I was able to immediately restart MySQL and the website immediately worked again

    $ service mysql restart

I hoped that this would be a one-time error. However, on two consecutive mornings soon after, I clicked on the site and the error was displayed again. I did some research and it turned out that XML-RPC attacks were being carried out against the website. This is a malicious, "brute force" attack in which thousands of requests are sent to a website in a short amount of time, rendering the database unresponsive. The attacks began around the time the website showed up in the Google search engine results and it seemed likely the attack crawler found it that way. I used guidance from the Digital Ocean article [How To Protect WordPress from XML-RPC Attacks on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-protect-wordpress-from-xml-rpc-attacks-on-ubuntu-14-04) to protect the website from this kind of attack. 

I first accessed the log to verify that an attack of this type had been carried out against the site

    $ grep xmlrpc /var/log/apache2/access.log

The log showed that thousands of attack requests had taken place. The Digital Ocean article explains how to identify an attack request. This is a screenshot of just a few of the many requests.

![](deployment-images/terminal-log-scrolling.jpg)

I then put a fix in place to block this kind of attack 

    $ sudo a2enconf block-xmlrpc

Finally, I restarted Apache for the fix to take effect

    $ sudo service apache2 restart

Though the attack requests would continue to show in the log, the code would no longer be 200. 

Apparently, the current configuration can be undone by using the following commands

    $ sudo a2disconf block-xmlrpc
    $ sudo service apache2 restart

One caveat to the approach that I used is that any service that utilizes XML-RPC will be prevented from functioning, including Jetpack or the WordPress mobile app. Though this website is not using Jetpack, if it were, another option would be to use a Jetpack security feature as outlined in the article [Jetpack Protection From Brute Force XML-RPC Attacks](https://jetpack.com/2015/10/12/jetpack-protection-from-brute-force-xml-rpc-attacks).

For further reference:
* ["Error establishing a database connection (WordPress)"](https://www.digitalocean.com/community/questions/error-establishing-a-database-connection-wordpress)
* [Jetpack Security Features](https://jetpack.com/support/security-features)
* [Sucuru "Brute Force Amplification Attacks Against WordPress XMLRPC"](https://blog.sucuri.net/2015/10/brute-force-amplification-attacks-against-wordpress-xmlrpc.html)
