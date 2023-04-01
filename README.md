# DNS_SERVER_APACHE

Here is an example of how to set up a DNS server using BIND and map another domain to an Apache server:

    Install BIND

sql

sudo apt-get update
sudo apt-get install bind9

    Configure BIND

bash

sudo nano /etc/bind/named.conf.local

Add the following contents to the file:

bash

zone "example.com" {
    type master;
    file "/etc/bind/db.example.com";
};

Save and close the file.

    Create DNS zone file

bash

sudo nano /etc/bind/db.example.com

Add the following contents to the file:

less

$TTL    604800
@       IN      SOA     ns1.example.com. admin.example.com. (
                  1         ; Serial
             604800         ; Refresh
              86400         ; Retry
            2419200         ; Expire
             604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.example.com.
@       IN      A       <IP address of your DNS server>
www     IN      A       <IP address of your Apache server>

Replace example.com with your domain name, <IP address of your DNS server> with the IP address of your DNS server, and <IP address of your Apache server> with the IP address of your Apache server.

Save and close the file.

    Restart BIND

sudo systemctl restart bind9

    Map another domain to your Apache server

You will need to add a VirtualHost configuration to your Apache server to map the domain to your Apache server. Here is an example configuration:

bash

sudo nano /etc/apache2/sites-available/anotherdomain.com.conf

Add the following contents to the file:

less

<VirtualHost *:80>
    ServerName anotherdomain.com
    DocumentRoot /var/www/html

    <Directory /var/www/html>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

Replace anotherdomain.com with the domain name you want to map to your Apache server. Save and close the file.

    Enable the new virtual host and restart Apache

sudo a2ensite anotherdomain.com.conf
sudo systemctl reload apache2

Your DNS server should now be set up and another domain should be mapped to your Apache server. Note that it may take some time for DNS changes to propagate, so you may need to wait a bit before testing the new domain mapping.
