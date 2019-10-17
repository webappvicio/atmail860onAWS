# atmail 8.6.0 on AWS
Installation procedure to install atmail 8.6.0 using AWS EC2, RDS, CloudWatch*, ElasticCache*.
Atmail is a mailserver, webmail suite that rule and control a large set of open sources technologies to run a state-of-the-art mailserver installation. 

#### Open Source packages out of the box with AtMail installation:
- _exim_: mail transfer agent (mta) handling smtp protocol for sending/receiving email messages.
- _dovecot_: imap/pop3 server to access email messages on mailserver (include Pigeonhole/Sieve filter/rules language)
- _clamav_: antivirus
- _spam assassin_: e-mail spam filtering
- _php-fpm, nginx_: application server and https server for web-admin and web-mail interface
- _sabredav_: dav(file storage), caldav (calendar), carddav(contacts) server
- _redis_: nosql based for user session management (to be replaced by AWS ElasticCache)
- _ansible_: configuration management and package depolyment tool
- _logstash_: logs processing and transformation (not installed, replaced by CloudWatch Agent)
- _elastic search_: search and analytics engine for events reporting and statistics (not installed, replaced by CloudWatch Inisghts)
- _mariadb_: open source fork of MySQL relational databases (not installed, replaced by AWS RDS)

#### Open Source add-on packages
- _fail2ban, firewalld_: intrusion prevention software framework that protects computer servers from brute-force attacks
- _impasync_: IMAP transfers tool

#### AtMail proprietary packages
- _atmail api server_: rest api to integrate
- _webmail app_
- _mailserver web admin interface_

### Requirements and 
- AWS account: you can setup a new account to take advantage of free tiers for many services
- 1 License from AtMail MailServer + AtMail Suite: minimum purchase for 357users + essential help costs 1450$ (4$ per user per year, 0.33$ per user per month)
- *my_domain*: 1 domain to identify mailserver (mail.example.com, mx.example.com) : you can purchase on AWS Route 53
- *my_admin*: 1 username for admin 
- *my_password*: 1 strong password 
- :large_orange_diamond: this symbol remarks patches to fix/improve compatibility of current AtMail installer or installation procedure

## 1. Launch RDS Instance

Launch RDS with MariaDB engine 
![](snapshot_1RDSMariaDB.png)

If you want to access to the RDS Instance directly from your computer, be sure to configure
*Public Available: True* and restart

## 2. Launch EC2, associate Elastic IP, request Reverse PTR

