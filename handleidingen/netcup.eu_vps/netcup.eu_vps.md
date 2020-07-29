* Datum: 2020-07-29
* Door: uOnline, Roger Sijben & Dion Dresschers
* Inspiratie: 
    * Eigen Brein
* Revisie: Jaarlijks
* Versie: 2020-07-29 11:28:19

troep dit

---

# netcup VPS

## netcup VPS - Observaties

Opgemerkt tijdens het uitvoeren van de onderstaande stappen.

1. Bij het maken van een offline snapshot, wordt de server kort gestopt en je verliest natuurlijk ook de SSH verbinding|
1. Na het terug zetten van een Snapshot, moet je zelf de server aanzetten (als je snel klikt, ben je binnen 35 Request timeout Pings weer terug online). Tip voor nieuwekomers: maak veel Snapshots en probeer veel uit.

## netcup VPS - Nieuwe Server Aanvragen

Aanloggen op onderstaande site van onze hosting provider netcup.eu

1. Ga naar `www.customercontrolpanel.de`
1. Ga naar `Products` en selecteer `Order`
1. Je ziet de tekst `Login successful` en kiest `Server`
1. Selecteer `Order now >` onder de te bestellen server (normaal zal dit een VPS 200 G8 zijn voor Standaard en een VPS 1000 G9 zijn voor Professioneel).
1. Selecteer `Add to cart >`|
1. Via `< Continue purchase, I want to complete the order later.` kan je meerdere servers te gelijk bestellen.
1. Zijn alle benodigde servers gekozen, kies dan `Open cart to complete purchase. >`.
1. Controleer de selectie, gebruik `X delete` of voeg producten toe, indien nodig, selecteer `Continue order >` als de bestelling compleet is.
1. Controleer nogmaals de geselecteerde servers en de `uOnline gegevens, selecteer `I have read and understood these General Terms & Conditiond (T&Cs) in full. I agree with this agreement and accept it.`, selecteer `Continue order`.
1. Een mail wordt verzonden naar `info` mailbox vanaf `netcup GmbH` met als onderwerk `Your order with netcup`.

## netcup VPS - Nieuwe Server connecteren

Voor onderstaande stappen uit, na ontvangst van de email van `server@netcup.de` met onderwerp "Ihr vServer bei netcup ist bereit gestellt" in de `info` mailbox
1. Onthoud uit de email de volgende regel `Hostname:  {v9999999999999999999.xxxxsrv.de}`, dit is het unieke nummer waar de server altijd onder terug te vinden is
1. Ga naar `www.servercontrolpanel.de`
1. Open de Server met nummer {v9999999999999999999}
1. Kies `Media` en selecteer `Images`
1. Als deze stappen wordt gevolgt, voor het herinstalleren van een bestaande server, dan kan deze melding verschijnen `Before setting up a new Image, all snapshots must be deleted.` (Kies `Snapshots` en gebruik `X delete` om alle bestaande Snapshots te verwijderen)
1. Kies `Ubuntu 20.04 LTS`
1. Kies `Minimal`
1. Kies `one big partition with os as root partition. Installation may take longer to finish. >`
1. Vergeet niet `send e-mail to me` aan te zetten `V`.
1. Vul het `SCP login password` in.
1. Selecteer `reinstall >`.
1. De volgende melding verschijnen:
   ```
   Server stopped,The selected Image was installed successfully.
   Server started,installation running
   ```
1. Een `VNC Console` popup scherm, toont de console installatie.
1. De installatie duurt ongeveer:
    * 6 minuten voor Ubuntu 18.04 (op een `VPS 2000 G8`)
    * 4 minuten voor Ubuntu 20.04 (op een `VPS 1000 G9`)
1. Wacht op de ontvangst van deze email `Neuinstallation Ihres Servers`
1. Sla de "English version:" van de email op in de `uOnlineBewaarBox`, onder:
    * "Klant" = {Klant Naam}
    * "VPS {99} met de volgende gegevens:
        * "Title" = {v9999999999999999999.xxxxsrv.de}
        * "Username" = "root"
        * "Password" = {The new root password:}
        * "URL" = "IP:"
1. Maak een "SSH" verbinding met het IP address|
1. Update alle voor geïnstalleerde software pakketten:
    * `sudo apt update`
    * `sudo apt upgrade`
1. Maak een `Offline Snapshot` in de `ServerControlPanel`, met de volgende gegevens:
    * `name` = `Ubuntu2004"
    * `description` = `Schone Installatie`
1. De volgende melding verschijnen:
    * `Server stopped`
    * `snapshot created succesfully`
    * `Server started`

## netcup VPS - Nieuwe Server Basis Instellingen

Gebruik onderstaande instellingen, om de server aan te passen voor gebruik.

1. Maak een `SSH` verbinding met het IP adress:
    * `ssh {ip address}`
1. Pas de hostname aan van de server, kies een bijpassende planeet uit deze lijst [List of Star Wars planets and moons - Wikipedia](https://en.wikipedia.org/wiki/List_of_Star_Wars_planets_and_moons)
    * `sudo hostnamectl set-hostname {Planneetnaam}`
1. Pas de local hostfile aan zodat de nieuwe hostnaam linkt naar het loopback adress:
    * `sudo vi /etc/hosts`
1. Vervang `{v9999999999999999999}` voor `{Planneetnaam}`
1. Maak in `gandi.net` een Type A DNS record aan:
    * `firefox http://gandi.net`
1. Vul de volgende settings in:
    ```
    Type - A
    Name - {planeetnaam}
    IPv4 Address - {ip adres}
    ```
1. Klik op `Create`
1. Herstart de server:
    * `sudo reboot`
1. Pas de hostname aan in de `uOnlineBewaarBox`
, onder
    * `{Klant Naam}` `VPS {99}`
1. Pas de gegevens aan:
    * `Title` = `{Planneetnaam}`
1. Pas in de `SCP` onder `General`, de `Nick` van de server aan naar de {Planneetnaam}
1. Omdat het `root` wachtwoord door netcup wordt gezet en wordt gemaild, is het aan te raden om deze aan te passen. `Generate` een nieuw 16 karakter lang wachtwoord in de `uOnlineBewaarBox` en kopieer deze naar de server, gebruik het volgende commando: `passwd`. Tip: test eerst de copy/past, voordat het passwd commando wordt gegeven.

## netcup VPS - Apache2

Als alle voorgaande hoofdstukken zijn uitgevoerd, maak dan een nieuwe "Offline Snapshot" in de "ServerControlPanel", met de volgende gegevens:

    * `name` = `Ubuntu2004PreWP`
    * `description` = `Voor WP Installatie`

[Wordpress adviseert](https://wordpress.org/download/) per 2020-06-28 het volgende:

* WordPress 5.4.2
* PHP 7.4
* MySQL 5.6 (of MariaDB 10.1)
* Apache2 (of NginX)

## netcup VPS - Apache2

1. Controlleer of Apache2 geinstalleerd is:
    * `dpkg -i | grep apache`
1. Zijn er geen resultaten zoek dan naar apache2 installaties:
    * `sudo apt search apache2 | grep ^apache`
1. Installeer het apache2 pakket:
    * `sudo apt install apache2`
1. Controlleer of apache2 draait:
    * `sudo systemctl status apache2`
1. Probeer of je de server kan bereiken via je lokale computer:
    * `firefox http://{servernaam}.uonline.nl`
1. Controlleer of de Apache2 PHP module actief is:
    * `a2query -m | grep php`
1. Controlleer welke Apache2 PHP geinstalleerd kan worden:
    * `sudo apt search apache2 | grep php`
1. Installeer de Apache2 PHP module
    * `sudo apt install libapache2-mod-php7.4`
1. Controlleer of de Apache2 PHP module actief is:
    * `a2query -m | grep php`
1. Installeer de PHP MySQL dirver:
    * `apt install php7.4-mysql`

## netcup vps - MariaDB

1. Controlleer of MariaDB actief is:
    * `systemctl status mysql`
1. Controlleer welke MariaDB installatiepakketten er zijn:
    * `apt search mysql | grep ^mysql`
1. Installeer de MariaDB server:
    * `apt install mysql-server`
1. Check of de server draait:
    * `systemctl status mysql`

### netcup vps - MariaDB - Installatie

1. Stel het MariaDB wachtwoord in:
    `sudo mysql_secure_installation`
1. Vul in bij `VALIDATE PASSWORD COMPONENT {...} .
    * [y]-toets
1. Vul in bij `password validation policy`
    * [2]-toets
1. Vul in bij `New password:`
    * {wachtwoord}
1. Vul in bij `Do you wish to continue with the password provide?`
    * [y]-toets
1. Maak in de "uOnlineBewaarBox", onder {Klant Naam} - {VPS} {99} het volgende aan:
    * `Title` = `MariaDB`
    * `Username` = `{wachtwoord}
1. Beantwoord `Remove anonymous users`:
    * [y]-toets
1. Beantwoord `Disallow root login remotely` 
    * [y]-toets
1. Beantwoord `Remove test database and access to it?`
    * [y]-toets
1. Beantwoord `Reload privilege tables now`
    * [y]-toets
1. Test de log in met het nieuwe wachtwoord voor `root`
    * `mysql -u root -p`
1. Test of de MySQL versie recent genoeg is voor WordPress:
    * `select version();`

### netcup vps - MariaDB - Nieuwe database en gebruiker aanmaken voor WordPress

1. Maak een nieuwe database aan:
    * `create database wordpressdb;`
1. Maak een nieuwe database gebruiker aan voor WordPress:
    * `create user 'wpdbuser'@'localhost' identified by '{wachtwoord}';`
1. Geef de gebruiker alle rechten op de database:
    `grant all on wordpressdb.* TO 'wpdbuser'@'localhost' WITH GRANT OPTION;`
1. Flush priviliges:
    * `flush privileges;`
1. Controlleer de rechten voor de gebruiker:
    * `show privileges for {wpdbuser}@localhost`
1. Exit:
    * `exit;`

## netcup VPS - WordPress installatie

1. Ga naar de `/tmp` directory:
    * `cd /tmp`
1. Download de laatse versie van WordPress:
    * `wget https://wordpress.org/latest.tar.gz` 
1. Pak het gedownloade bestand uit:
    *  `tar -zxvf /tar/latest.tar.gz`
1. Verwijder de `/var/www/html/` directory:
    * `rm -rf /var/www/html`
1. Verplaats de `wordpress` directory naar `/var/www/html`
    * `sudo mv /tmp/wordpress /var/www/html/`
1. Verwijder de gedownloade download:
    * `rm /tmp/latest.tar.gz`
1. Stel nu de juiste eigenaar in:
    `sudo chown -R www-data:www-data /var/www/html`
1. Stel de juiste rechten in:    
    `sudo chmod -R 755 /var/www/html`
1. Zet de juiste gegevens in de `wp-config.php`-file:
    * `sudo mv /var/www/html/wordpress/wp-config-sample.php /var/www/html/wp-config.php`
    * `sudo vi /var/www/html/wp-config.php`
1. Verander deze 3 regels:
    ```
    define( 'DB_NAME', '{wpdatabase}' );
    define( 'DB_USER', '{wpdatabasegebruiker}' );
    define( 'DB_PASSWORD', '{wpdatabasewachtwoord}';
    ```
1. Open WordPress:
    `firefox http://{servernaam}.uonline.nl`
1. Kies
    * `Nederlands (Formeel)`
    * `Continue`
1. Vul de volgende gegevens in:
    * `Site Title` = `{WordPressTitel}`
    * `Username` = `{WordPress Admin Gebruiker sla op in de uOnlineBewaarBox}`
    * `Password` = `{Sla op in de uOnlineBewaarBox}`
    * `Your Email` = `{e-mail adres}`
1. Klik op `Install WordPress`
1. Login met de net aangemaakte WordPress 'admin' gebruiker.
1. Controleer of er `Updates` zijn, die geïnstalleerd kunnen worden.
1. Pas onder `Settings`, `General` de volgende gegevens aan:
    * `Site Language` - `"Nederlands (Formeel)`
    * `Date Format` - `Y-m-d`
    * `Time Format` - `H:i`
1. Maak voor de zekerheid een VPS snapshot.

