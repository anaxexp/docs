# Infrastructure maintenance

We constantly improve the infrastructure we deploy to our customers' servers. You can see the version of the infrastructure deployed to your server in the Dashboard on the servers list page. It's not always possible to update the infrastructure automatically so if you want update your infrastructure please [contact our support team](../support.md) to schedule the upgrade. 

!!! success "Supported version"
    Currently supported infrastructure version is [5.5.0+](#550)

You will be notified each time a new version of the infrastructure is released.

!!! tldr "Stacks and infrastructure maintained separately"
    For stacks maintenance see [this article](../stacks/maintenance.md) 

## Changelog

### 5.5.5

* Bugfix (5.5.4): installer did not report changes to IP address

### 5.5.4

* Updates to installer, you don't need to update from the previous version

### 5.5.3

* [HSTS](hsts.md) can now be configured per domain

### 5.5.2

* Security vulnerability fix for dnsmasq [CVE-2017-14491](https://security.googleblog.com/2017/10/behind-masq-yet-more-dns-and-dhcp.html)

### 5.5.1

* Increased kernel param `aio-max-nr` for databases
* Updated paths for systemd unit files
* Uninstall fixes

### 5.5.0

* New Docker 17.06.1
* Improved response codes on Edge

### 5.4.1

* Updated [Let's Encrypt client](../apps/domains.md)

### 5.4.0

* AUFS replaced with Overlay2
* Identified a bug in Debian Kernel 3.16, required upgrade to 4.9
* Fixed a bug with kube-controller service definition that sometime caused deployment failures

### 5.3.0

* New kubernetes version
* Improved agent installer
* Enabled unattended upgrades 
* Significantly improved performance (less load on CPU and disk IO)

### 5.2.0

* Decoupled services for system containers
* Improved agent installer
* Bug fixes

### 5.1.0

* Revamped DNS services
* Improved agent installer
* Bug fixes

### 5.0.0

* New kubernetes
* New installer with frozen docker version
* Revamped anaxexp agent

### 4.x

First version of cluster infrastructure

### 3.5.0

* Updated Nginx (1.10.1) for a system container Edge  
* New version of AnaxExp agent supporting containers upgrade
* New version of orchestration system

Release date: `July 1st, 2016`

### 3.4.0

* `includeSubdomains` option removed from [HSTS header](hsts.md)
* Now `X-Robots-Tag` header added always (not only for 20x, 30x response codes)
* New version of AnaxExp agent. Now with automated infrastructure update (will be announced later)
* Exif extension added to PHP 5.6, 7
* Fixed bug when `X-AnaxExp-Node` header missed sometimes
* Added default nginx host for port 443 with self-signed certificates 

Release date: `June 16th, 2016`

### 3.3.0

* $base_url orchestration for Drupal
* Auto generation of trusted host patterns for Drupal 8
* Drupal 7.x, 8.x multi-site support
* Drupal 8 and WordPress now come with PHP7 and Redis
* msmtp + opensmtpd replaced with postfix
* Workaround for Drupal `sites/default` auto permissions change. This caused problems when settings.php file was changed

### 3.2.0

* WWW redirect actions for domains
* Basic auth configuration
* Maintenance mode
* Dev, staging instances and all instances accessible by technical `*.wod.by` domains not indexed by search engines (header X-Robots-Tag)  

### 3.1.0

* Enable HTTPS for domains (SSL certificates via Let's Encrypt) 
* Backup mirroring features added.

### 3.0.0

The latest stable version with completely reworked containers structure.

### 2.0.0

In this version we've moved git to the docroot and made major structural changes.

### 1.0.0

Production-ready version with lots of improvements.

### 0.1.0

The first public version of our infrastructure.
