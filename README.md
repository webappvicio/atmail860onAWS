# atmail 8.6.0 on AWS
Installation procedure to install atmail 8.6.0 using AWS EC2, RDS, CloudWatch*, ElasticCache*.
Atmail is a mailserver, webmail suite that rule and control a large set of open sources technologies to run a state-of-the-art mailserver installation. 

Open Source packages out of the box with AtMail installation:
- exim: mail transfer agent (mta) handling smtp protocol for sending/receiving email messages.
- dovecot: imap/pop3 server to access email messages on mailserver (include Pigeonhole/Sieve filter/rules language)
- clamav: antivirus
- spam assassin: e-mail spam filtering
- php-fpm, nginx: application server and https server for web-admin and web-mail interface
- sabredav: dav(file storage), caldav (calendar), carddav(contacts) server
- redis: nosql based for user session management (to be replaced by AWS ElasticCache)
- ansible: configuration management and packge depolyment tool
- logstash: logs processing and transformation (not installed, replaced by CloudWatch Agent)
- elastic search: search and analytics engine for events reporting and statistics (not installed, replaced by CloudWatch Inisghts)
- mariadb: open source fork of MySQL relational databases (not installed, replaced by AWS RDS)

Open Source add-on packages
- fail2ban, firewalld: intrusion prevention software framework that protects computer servers from brute-force attacks
- impasync: IMAP transfers tool

AtMail proprietary packages
- atmail api server: rest api to integrate
- webmail apps
- mailserver admin interface



