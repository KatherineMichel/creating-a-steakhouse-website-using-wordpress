# Deployment

## Primary Software

* Domain ([www.prettyprairiesteakhouse.com](http://www.prettyprairiesteakhouse.com)): [GoDaddy Domain Name Search](https://www.godaddy.com/domains/domain-name-search)
* Domain private registration: [GoDaddy Private Registration](https://www.godaddy.com/domainaddon/private-registration.aspx)
* Host: [Digital Ocean](https://www.digitalocean.com)
* Theme: [Sage from HighGrade Lab](http://www.highgradelab.com/sage)
* SSH client: [PuTTY](http://www.putty.org) and Mac terminal

## SEO

* Webmaster: [Google Webmaster Tools](https://www.google.com/webmasters/tools)
* General SEO: [Yoast SEO for WordPress Plugin](https://yoast.com/wordpress/plugins/seo)
* XML Sitemap Generator: [Google XML Sitemap Generator Plugin](https://wordpress.org/plugins/google-sitemap-generator)

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
* Set up [Digital Ocean One-Click Install](https://www.digitalocean.com/community/tutorials/how-to-use-the-wordpress-one-click-install-on-digitalocean)
* [Configured GoDaddy domain to point to Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars)
* [Configured domain in Digital Ocean control panel](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean)
* Enabled installation by [connecting to droplet with SSH](https://www.digitalocean.com/community/tutorials/how-to-connect-to-your-droplet-with-ssh) via PuTTY and Mac terminal, changing password
* Created special Apache server configuration via SSH and nano to enable upload of larger premium theme
* Used local environment to test WordPress before implementing in host environment
* Used Google Canary Chrome Development Tools [Device Mode](https://developers.google.com/web/tools/chrome-devtools/iterate/device-mode/?hl=en) for device simulation for testing; also crowdsourced device testing
* Used [Yoast SEO for WordPress Plugin](https://yoast.com/wordpress/plugins/seo), [Google XML Sitemap Generator Plugin](https://wordpress.org/plugins/google-sitemap-generator), and [Google Webmaster Tools](https://www.google.com/webmasters/tools) for search indexing

Digital Ocean Specs

    WordPress on 14.04
    $10/month $0.015/per hour
    1 GB/1 CPU
    30 GB SSD Disk
    2 TB Transfer
    Datacenter region: New York
    Backup

## Trouble Shooting

### Theme Installation

Apache server file access via SSH and nano

    $ sudo nano /etc/php5/apache2/php.ini

Altered settings

    upload_max_filesize = 50M
    post_max_size = 50M
    memory_limit = 200M
    max_input_time = 150

And server restart

    $ service apache2 restart

I was then able to upload and activate the theme without incident. 

### Downtime

I was searching for the website daily in Google in anticipation that it would show up in the results. When it did, I clicked on the site and it displayed an error. 

I was able to immediately restart MySQL and the website immediately worked again

    $ service mysql restart

I hoped that this would be a one-time error. However, twice in the next few days, in the morning I clicked on the site and the error was displayed again. 

https://www.digitalocean.com/community/questions/error-establishing-a-database-connection-wordpress
https://www.digitalocean.com/community/tutorials/how-to-protect-wordpress-from-xml-rpc-attacks-on-ubuntu-14-04
https://jetpack.com/2015/10/12/jetpack-protection-from-brute-force-xml-rpc-attacks/
https://jetpack.com/support/security-features

MySQL Restart

    $ grep xmlrpc /var/log/apache2/access.log

    $ sudo a2enconf block-xmlrpc
    $ sudo service apache2 restart
