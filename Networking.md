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

  
# 2) 

