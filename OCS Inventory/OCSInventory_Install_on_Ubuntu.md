# OCS inventory paigaldamine Ubuntule 22.04.


## Vajalikud moodulid.
- Apache2
  - Mod_Perl Version 1.29
- PHP 7 or higher, with ZIP and GD support enabled.
  - php_curl
  - php_mbstring
  - php_soap
  - php_xml
- PERL 5.6 or higher.
  - Perl module XML::Simple version 2.12 or higher.
  - Perl module Compress::Zlib version 1.33 or higher.
  - Perl module DBI version 1.40 or higher.
  - Perl module DBD::Mysql version 2.9004 or higher.
  - Perl module Apache::DBI version 0.93 or higher.
  - Perl module Net::IP version 1.21 or higher.
  - Perl module SOAP::Lite version 0.66 or higher (optional)
  - Perl module Mojolicious::Lite
  - Perl module Plack::Handler
  - Perl module Archive::Zip
  - Perl module YAML
  - Perl module XML::Entities
  - Perl module Switch
- MariaDB version 4.1.0 or higher
- Make utility 
## 1. Seadista vajalikud moodulid.

### 1.2. Uuendused, andmebaas ja basic moodulid.
Et kõik repositorid oleksid värsked, käivita järgnev käsk:
```
sudo apt update
```
Paigaldame MariaDB, Selleks käivita järgnev käsk:
```
sudo apt install mariadb-server mariadb-client
```
Ja paigalda süsteemi vajalikud tööriistad:
```
sudo apt install git curl wget make cmake gcc make
```
## 2. Seadista Apache veebi server
Et seadistada Apache veebiserver Ubuntule, käivita käsureal järgev käsk:
```
sudo apt -y install libapache2-mod-perl2 libapache-dbi-perl libapache-db-perl libapache2-mod-php libarchive-zip-perl
```
## 3. Paigalda PHP ja PHP moodulid.
Kuna Ubuntu 22.04 versioonil on on default PHP versioon 8.0 siis paigaldame selle.
Seadistame PHP8 ja OCS serveri joaks vajalikud moodulid, selleks käivita järgnev käsk:
```
sudo apt -y install php php-zip php-pclzip php-gd php-soap php-curl php-json php-mbsting php-xml php-mysql
```
## 4. Perl paigaldus ja kõik vajalikud moodulid.
Perl ja vajalike moodulite paigaldamiseks käivita järgnevad käskud:
```
sudo apt -y install perl libxml-simple-perl libcompress-zlib-perl libdbi-perl libdbd-mysql-perl libnet-ip-perl libsoap-lite-perl libio-compress-perl libapache-dbi-perl  libapache2-mod-perl2 libapache2-mod-perl2-dev
```
Seadista moodulid:
```
sudo perl -MCPAN -e 'install Apache2::SOAP'
```
```
sudo perl -MCPAN -e 'install XML::Entities'
```
```
sudo perl -MCPAN -e 'install Net::IP'
```
```
sudo perl -MCPAN -e 'install Apache::DBI'
```
```
sudo perl -MCPAN -e 'install Mojolicious'
```
```
sudo perl -MCPAN -e 'install Switch'
```
```
sudo perl -MCPAN -e 'install Plack::Handler'
```
## 5. Andmebaasi loomine.
Logi oma MariaDB-se sisse ja käivita seal järgnevad käsud:
```
sudo mysql -u root -p
```
Loome ocs andmebaasi:
```
CREATE DATABASE ocs;
```
Loome kasutaja ocs ja anname ocs andmebaasile õigused:
```
GRANT ALL PRIVILEGES ON ocs.* TO ocs IDENTIFIED BY "V2gaTurvalineParool";
```
```
FLUSH PRIVILEGES;
```
```
EXIT;
```
## 6. OCS inventory serveri paigaldamine.
Selleks et tõmmata OCS inventory tuleb võtta nende github lehelt värskeb väljalase.
>https://github.com/OCSInventory-NG/OCSInventory-ocsreports/releases/
Antud dokumentatsiooni loomise hetkel oli 2.12.0 versioon väljastatud.
```
wget https://github.com/OCSInventory-NG/OCSInventory-ocsreports/releases/download/2.12.0/OCSNG_UNIX_SERVER-2.12.0.tar.gz
```
```
tar -xf OCSNG_UNIX_SERVER-2.12.0.tar.gz
```
### 6.1 PHP composeri paigaldus
Tuleb paigalda composer ocs joaks, selleks käivita järgnevad käsud:
```
cd OCSNG_UNIX_SERVER-2.12.0
```
```
cd ocsreports
```
```
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```
ja navigeeri tagasi eelmisesse kausta
```
cd ..
```
### 6.2 OCS setup.sh faili eelseadistamine.
Muuda faili `setup.sh` sisu järgnevalt
```
# Which host run database server
DB_SERVER_HOST="${DB_SERVER_HOST:-localhost}"
# On which port run database server
DB_SERVER_PORT="${DB_SERVER_PORT:-3306}"
# Database server credentials
DB_SERVER_USER="${DB_SERVER_USER:-ocs}"
DB_SERVER_PWD="${DB_SERVER_PWD:-V2gaTurvalineParool}"
```
Kui sa kasutad eraldiseisvat andmebaasi serverit siis muuda `DB_SERVER_HOST:-` väärtus vastavaks.
Ja salvesta `setup.sh`fail
### 6.3 OCS installi käivitamine
Käivita paigalduse script
```
sudo ./setup.sh
```
Enamustele tuleb jätta default, kuid jälgi mida sul küsitakse. näiteks kui on vaja seadistada andmebaasi infot jne.

## 7. OCS Inventory Webserveri seadistus.
Navigeeri `/etc/apache2/conf-available` kataloogi ja muudame `z-ocsinventory-server.conf` ja `zz-ocsinventory-restapi.conf`





### Kasutatud allikad
> http://wiki.ocsinventory-ng.org/03.Basic-documentation/Setting-up-a-OCS-Inventory-Server/
