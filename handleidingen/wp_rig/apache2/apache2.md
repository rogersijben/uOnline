# Apache2 op Ubuntu 20.04

* Datum: 2020-07-29
* Door: uOnline
* Inspiratie: 
    * Eigen Brein
* Revisie: Geen
* Versie: 2020-07-29 11:28:19

---

## Apache2 - Installatie

1. Installeer Apache2
    * `apt install apache2`
1. Controlleer of apache draait:
    * `systemctl status apache2`
1. Bekijk de apache2 configuratie files:
    * `ls -l /etc/apache2`
1. Test of apache werkt door in Firefox te openen:
    * `firefox http://127.0.0.1`
1. Maak een PHP index file aan:
    * `vi /var/www/html/index.php`
1. Pas de inhoud van de `index.php` file aan zodat de inhoud is:
     
    ```
    <?php

    echo("Hallo vanuit PHP");
    ```

1. Bekijk de inhoud van het `index.php` bestand via het commando:     
    * `cat /var/www/html/index.php`

## Apache2 - Installeer PHP Module

1. Bekijk alle apache commando's via:
    * `a2` + [Tab]-toets
1. Het resultaat is:

    ```
    a2disconf  a2dismod   a2dissite  a2enconf   a2enmod    a2ensite   a2query    
    ```

1. Bekijk alle `a2query` commando's via:
    * `a2query`
1. Controlleer alle modules die aan staan:
    * `a2query -m`
1. Controlleer of de PHP module aan staat:
    * `a2query -m | grep php`
1. Controlleer of Apache2 PHP module te installeren is:
    * `apt search apache | grep php`
1. Installeer de Apache PHP module:
    * `apt install libapache2-mod-php7.4`
1. Controlleer of de PHP module aan staat:
    * `a2query -m | grep php`
1. Het resultaat is:
    * `php7.4 (enabled by maintainer script)`
1. Controlleer of PHP nu wel werkt:
    * `firefox http://127.0.0.1/index.php`
    
## Apache2 - Maak nieuwe virtual host aan

1. Maak een niewe direcory bijvoorbeeld `site_010_wordpress` aan waar je files komen:
    * `mkdir /var/www/html/<site_010_wordpress>`
1. Ga naar de Apache2 config directory:
    * `cd /etc/apache2/
1. Copieer de standaar config file naar de nieuwe virtual host:
    * `cp sites-available/000-default.conf sites-avaiblable/010-wordpress.conf`
1. Pas de configuratie aan van de nieuwe config file:
    * `vi sites-available/010-wordpress.conf`
1. Zodat het resultaat is:

    ```
    <VirtualHost 127.0.1.1:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html/site_010_wordpress

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>

    # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
    ```
1. Maak de file:
    `vi /var/www/html/site_010_wordpress/index.html1`
1. Zodat de inhoud is:
    ```
    <!DOCTYPE html>
    <html>
    <head>
    </head>
    <body>
    Hallo vanuit HTML
    </body>
    </html>
    ```
1. Bekijk alle Virtual Host config files:
    * `ansite`
1. Maak de nieuwe Virtual Host actief
    * `a2ensite 010-wordpress.conf`
1. Herstart Apache2
    * `systemctl restart apache2`
1. Controlleer of de nieuwe site werkt:
    * `firefox http:127.0.1.1`
