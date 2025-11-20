# Compute

## Lab 3 - Introduction to Amazon EC2


### 1. EC2 instanssit välilehdeltä käynnistin Web palvelimen missä termination protection päällä

Kun se ilmestyi Instances välilehdelle, katsoin, että Status on 2/2

Tarkkailin myös Monitorin välilehdeltä eri asioita, esim virtuaalikoneen CPU utilization yms.

Katsoin myös System Logeja jossa näkyisi esim. mahdolliset ongelmat jos niitä olisi.

### 2. Muokkasin Security Groupin asetuksia

Kun laitoin instanssin julkisen ipv4 osoitteen hakukenttään, se ei löytänyt sivua

Avasin inbound rulesista portin 80, sillä siellä ei ollut sääntöä joka sallsi HTTP-liikenteen porttiin 80

### 3. Muutin EC2 instanssin kokoa, sekä laitoin Stop Protectionin päälle

Ensiksi sammutin instanssin, sitten muokkasin Modify Volume välilvehdeltä sen kokoa 8GiB --> 10GiB

Sitten Actions väliehdeltä Stop Protection, ja sieltä enable

### 4. Instanssi pois päältä 

Kokeilin sitten sammuttaa kyseistä instanssia, mutta se ei tietenkään onnistunut Stop Protectionin takia

Vaihdoin sen takaisin pois enable tilasta, ja sammutin sen, ja se sammui.

### Yhteenveto AWS

Tässä labrassa opin, että EC2-instanssin hallinta vaatii oikeiden verkkoasetusten kuten Security Groupin porttien
sekä suojauksien kuten Stop Protectionin huomioimista. Instanssin tilaa voi seurata eri välilehdiltä, kuten status, monitor ja system log. 
Levytilaa voi muuttaa myös muuttaa sammutetusta instanssista, ja Stop Protection estää vahingossa tapahtuvan sammutuksen, 
mutta sen voi tarvittaessa poistaa, jotta instanssi voidaan sammuttaa. Lisäksi AWS:n Service Quotas kohdasta voi tarkistaa, 
kuinka monta On-Demand EC2 -instanssia voi samanaikaisesti ajaa, mikä auttaa hallitsemaan resurssien käyttöä ja välttämään rajoitusten ylityksen.


## Create an Azure Virtual Machine

### 1. Virtuaalikone

Avasin ensiksi chromessa Azuren portaalin, kirjauduin oikealle käyttäjälle 

Avasin Cloud Shellin, valitsin bash ja laitoin oikeat asetukset 

Sitten ajoin seuraavan komennon:

- Eli luodaan virtuaalikone
- Määritimme resurssiryhmän johon virtuaalikone luotiin
- Virtuaalikoneelle nimi
- Käyttöjärjestelmä "image"
- Ylläpitäjän käyttäjänimi jolla sinne kirjaudutaan
- SSH avaimet automaattisesti kirjautumista varten 

az vm create \
--resource-group myRGKV-lod56866572 \
--name my-VM-56866572 \
--image Ubuntu2204 \
--admin-username azureuser \
--generate-ssh-keys

### 2. Virtuaalikoneen laajennus

Sitten laajennus joka lataa ja suorittaa skriptin Nginxin asentamiseksi ja konfiguroimiseksi:

- Azure virtuaalikoneen laajennus
- Resurssiryhmä johon virtuaalikone kuuluu
- Virtuaalikoneen nimi johon laajennus tulee
- Asennettavan laajennuksen nimi
- Julkaisija ja versio
- Skriptin url joka ajetaan virtuaalikoneella, urlissa siis skripti jolla
  asennetaan ja konfiguroidaan Nginx
- Määritys komennolle, joka ajetaan skriptin ajamisen jälkeen
- Lopuksi asetus, jotta avaimet eivät näy selkokielisinä Azure-portaalissa


az vm extension set \
--resource-group myRGKV-lod56866572 \
--vm-name my-VM-56866572 \
--name customScript \
--publisher Microsoft.Azure.Extensions \
--version 2.1 \
--settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
--protected-settings '{"commandToExecute": "./configure-nginx.sh"}'

Msle.learnondemandin nettisivun mukaan labra on suoritettu, mutta siinä ei näy samalla tavalla arvosanaa, kuten vaikka aiemmin tehdyssäni Azuren labrassa, 
joka mietityttää.

## Yhteenveto molemmista labroista

Näissä labroissa "opin" hallitsemaan jotenkin AWS:n EC2-instansseja ja Azure-virtuaalikoneita. AWS:ssä käynnistin Web-palvelimen, 
tarkkailin sen tilaa, muutin Security Groupin porttiasetuksia ja testasin Stop Protectionin vaikutusta, 
mikä havainnollisti hyvin instanssin suojauksien ja verkkoasetusten merkityksen. Azure-labrassa loin virtuaalikoneen, määritin SSH-avaimet ja resurssiryhmän,
sekä asensin ja konfiguroin Nginxin skriptillä laajennuksen avulla, ja tämä osoitti, miten VM:n konfigurointi voidaan automatisoida. 
Ero AWS:n ja Azuren välillä näkyi käyttöliittymässä ja hallintatavoissa, vaikka perusperiaatteet kuten instanssien tila, 
suojaus ja verkkoasetukset, ovat samankaltaisia.







