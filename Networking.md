# 1) Build your VPC and Launch a Web Server (AWS)

## Luodaan VPC ja Subnetit

#### Aluksi labrassa loin VPC:n, jossa käytännössä kaikki resurssini toimii, eli oma virtuaalinen eristetty "datakeskus" joka on kriittinen omien verkkojen, reititysten tai suojausasetusten määrittämistä varten

- Määritin yhden public subnetin, sekä yhden private subnetin, jotta voidaan erottaa julkiset ja yksityiset resurssit 

- AWS loi automaattisesti tarvittavat verkkoresurssit, kuten Internet Gateway, NAT Gateway sekä Public ja Private route tablet

- Myös DNS asetukset pidetään päällä, jotta instanssit voi käyttää DNS nimiä

--->

- Luomisen jälkeen sain kontrollin verkon IP-alueista
  
- Mahdollistaa turvallisen ja hallitun ympäristön rakentamisen instansseille

- Perusta kaikelle muulle verkkoarkkitehtuurille

- Myös määritetyt CIDR-blockit varmistaa selkeän jaon julkisen ja yksityisen verkon välillä
  

<img width="1189" height="175" alt="Screenshot 2025-11-28 at 17 07 24" src="https://github.com/user-attachments/assets/d9592a9d-52c0-4d31-a548-652590dcb59f" />


## VPC Security Group

#### Siirryin Security Group osioon ja loin uuden nimeltä Web Security Group

- Valitsin oikean VPC:n eli lab-vpc, jotta kyseinen ryhmä kohdistuisi oikeaan verkkoon

#### Inbound säännöt:

- HTTP Port 80
- Source: Anywhere.IPv4 (0.0.0.0/0)

Näillä tarkoitus sallia selaimesta tulevat web-pyynnöt koko internetistä


<img width="1437" height="333" alt="Screenshot 2025-11-28 at 17 13 57" src="https://github.com/user-attachments/assets/52318ba1-b55e-4624-b4d1-c390576279e0" />

#### Kyseinen Security Group toimii EC2 instanssin "palomuurina"

##  Web-palvelininstanssin käynnistys 

#### EC2 konsoli auki ja uuden instanssin käynnistys nimeltä Web Server 1 

- AMI Amazon Linux 2023

- Instanssityyppi t2.micro, joka riittää suht kevyelle web-palvelimelle

- Vockey avain, jotta instanssiin saa SSH-yhteyden tarvittaessa

- VPC: lab-vpc

- Subnet: lab-subnet-public2, jotta saa julkisen osoitteen

- Auto-Assign public ip enabled

- Liitin instanssiin äsken tehdyn Web Security Groupin, joka siis sallii HTTP-liikenteen

#### Sitten user data kohtaan skripti joka:


- Asentaa Apache HTTP -palvelimen, PHP:n ja MariaDB:n

- Lataa labramateriaalit

- Käynnistää web-palvelimen automaattisesti


#### Odotin 2/2 checkit, ja sitten vierailin julkisessa osoitteessa


<img width="1169" height="312" alt="Screenshot 2025-11-28 at 17 29 29" src="https://github.com/user-attachments/assets/985d5282-b402-4ac3-9fde-216b8bcdb3cd" />


## Yhteenveto

#### Ymmärsin:

- Miten rakennetaan AWS:ssä perusverkkoympäristö (VPC) alusta asti

- Miten public ja private subnetit toimivat ja miksi niitä käytetään

- Miten Security Group luodaan ja mikä rooli mm. sillä on EC2-instanssien palomuurina

- EC2-instanssin käynnistämisen omassa VPC:ssä sekä verkkoasetusten ja tietoturvan määrittämisen

- Tärkeimpänä miten User data skripti automatisoi ohjelmistojen asennuksen ja web-palvelimen käyttöönoton

#### Lopputuloksena toimiva web-palvelin ja konkreettinen esimerkki että se toimii

  
# 2) Configure network access (Azure)

## Ensiksi hain VM:n julkisen ip osoitteen 

#### Ajoin komennon jolla hain julkisen ip osoitteen ja sitten tallensin ipn muuttujaan $IPADDRESS

Testasin Web palvelinta komennolla:

#### $ curl --connect-timeout 5 http://$IPADDRESS

- Ei toiminut koska NSG:ssä ei oltu avattu porttia 80

- Listasin VM:ään liittyvän Network Security Groupin (NSG)

- Siinä näkyi, että ainut sallittu sääntö oli "default allow SSH" eli portti 22

#### Loin uuden verkkoturvasäännön

Loin allow-http -säännön, joka sallii:

- Inbound

- Port 80

- TCP

- Source: *

- Priority: 100

-  Action: Allow

#### Uudelleenlistasin NSG:n ja näin:

default-allow-ssh   1000   22   Allow
allow-http          100    80   Allow


<img width="761" height="188" alt="Screenshot 2025-12-02 at 0 31 42" src="https://github.com/user-attachments/assets/db22d4fe-c9d5-4abd-8ad2-c2f247431df4" />

#### Portti 80 toimi ja sivu latautui cURL komennolla onnistuneesti 


<img width="1470" height="956" alt="Screenshot 2025-12-02 at 0 31 48" src="https://github.com/user-attachments/assets/b317dfc9-ce49-4534-8328-9f8d76a75c6d" />

## Yhteenveto

#### Tajusin miten Azure Virtual Network ja Network Security Group toimivat yhdessä suojatakseen virtuaalikoneita. Loin käytännössä oman palomuurisäännön allow-http, tajusin sen prioriteetit ja miten se vaikututtaa, sitten testasin että säädöt toimivat käytännössä avaamalla web-palvelimen portin 80, sekä vierailemalla sivulla.





