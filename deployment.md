# Deployment

## Primary Software

* Domain ([www.prettyprairiesteakhouse.com](http://www.prettyprairiesteakhouse.com)): [GoDaddy Domain Name Search](https://www.godaddy.com/domains/domain-name-search)
* Domain private registration: [GoDaddy Private Registration](https://www.godaddy.com/domainaddon/private-registration.aspx)
* Host: [Digital Ocean](https://www.digitalocean.com)
* Theme: [Sage from HighGrade Lab](http://www.highgradelab.com/sage)
* SSH client: [PuTTY](http://www.putty.org) and Mac terminal

## Utilities

* Photo shopping: [Gimp](https://www.gimp.org)
* Zipfile management [PeaZip](https://sourceforge.net/projects/peazip)
* Google Canary Chrome Development Tools [Device Mode](https://developers.google.com/web/tools/chrome-devtools/iterate/device-mode/?hl=en) for device simulation for testing

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
* Use of Google Canary Chrome for device simulation for testing; also crowdsourced device testing

Digital Ocean Specs

    WordPress on 14.04
    $10/month $0.015/per hour
    1 GB/1 CPU
    30 GB SSD Disk
    2 TB Transfer
    Datacenter region: New York
    Backup

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
