# atmail 8.6.0 on AWS
Installation procedure to install atmail 8.6.0 using AWS EC2, RDS, CloudWatch*, ElasticCache*.
Atmail is a mailserver, webmail suite that rule and control a large set of open sources technologies to run a state-of-the-art mailserver installation. 
The configuration aims to handle multiple domains with indipendent SSL certifciates for SMTP, IMAP, HTTPS.

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
- *AtMailID, License*: 1 License from AtMail MailServer + AtMail Suite: minimum purchase for 357users + essential help costs 1450$ (4$ per user per year, 0.33$ per user per month)
- *my_domain*: 1 domain to identify mailserver (mail.example.com, mx.example.com) : you can purchase on AWS Route 53
- *my_admin*: 1 username for admin 
- *my_password*: 1 strong password that we will use for all packages and installation
- :large_orange_diamond: this symbol remarks patches to fix/improve compatibility of current AtMail installer or installation procedure

## 1. Launch RDS Instance

Launch RDS with MariaDB engine 
![](snapshot_1RDSMariaDB.png)

If you want to access to the RDS Instance directly from your computer, be sure to configure
*Public Available: True* and restart. 

After RDS is launched please take notes of the IP address. This procedure will refer to RDS IP address as *my_rds*. 

:dollar: RDS separate daatabase allows to use a free tier resources and optimize the mail server workload, with also automatic backup, software update, monitoring and if necessary to increase the performance scaling up the RDs instance. 

## 2. Launch EC2 and security group

Launch EC2 Instance Amazon Linux 2 AMI (HVM), SSD Volume Type - 64-bit (x86). This kind of instance is the most optiimized, efficient, supported type of instance in the AWS uniserver and it's very easy to adap the AtMail install to work on Amazon Linux 2. 

We reccomend to select *t3.medium* instance to comply with the AtMail minimum requirement of 4GB of memory avialable.

:dollar: t3.medium EC2 as reserved instance (no-upfront) as a cost of 25$ per month (300$ per year, 0.85% per year per user on the 357 users license).

It's preferred to have 1 root volume with the operating system and the software (8GB-16GB), and 1 extra EBS volume to hold the imap data.

:pushpin: Evaluate to have 1 encrypted EBS for each domain configured in the mail server: dovecot/Mail_location. 

Create 2 security groups:
- _admin_ security group with all traffic open only for administration IP (your IP)
- _atmail_ security group with open ports for all traffic: smtp(25), smtp submission(587), smtps(465), imap(143), imaps(993), pop3(110), pop3s(995), http(80), https(443), dav(8008), davs(8443)

## 3. Associate Elastic IP, request Reverse PTR

Alocate a new Elastic IP address and associate with EC2 Instance. Verify that the IP is a 'clean' IP, and it's not already in a blacklist.

Check: https://mxtoolbox.com/blacklists.aspx

If the IP is in a blacklist, release the IP and request a new one. 

:rescue_worker_helmet: Reallocating a new IP is a fast recover procedure if accidentally the mailserver is caught in a blacklist.

It's important to configure the reverse DNS as identity proof for the mail server, and it's necessary to request to AWS. Usually AWS processes the request in a few hours: https://aws.amazon.com/forms/ec2-email-limit-rdns-request






