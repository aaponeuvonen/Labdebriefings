# 1) Introduction to AWS IAM

En ikävä kyllä jostain syystä ottanut screenshotteja tästä.

Labrassa tavoitteena ymmärtää käytännössä AWS IAM -palvelun perusteet: käyttäjien, ryhmien ja käyttöoikeuksien hallinta oikeassa AWS-ympäristössä

Vaiheet ohjeiden mukaan: 

### 1. Tutustuin ensiksi valmiiksi luotuihin IAM-käyttäjiin ja ryhmiin

Eri käyttäjiä oli: user-1, user-2, user-3

Ja eri ryhmiä: S3-Support, EC2-Support, EC2-Admin

### 2. Tarkastelin kyseisille ryhmille liitettyjä IAM-policeja (oikeuksia, menettelytapoja, politiikkoja)

S3-Support --> AmazonS3ReadOnlyAccess

EC2-Support --> AmazonEC2ReadOnlyAccess

EC2-Admin --> Oma Inline Policy (Start/Stop EC2 instances)


### 3. Sitten lisäsin käyttäjät oikeisiin ryhmiin liiketoimintatarpeiden mukaisesti kuten ohjeessa sanottiin 

user-1 --> S3-Support (S3 ReadOnly)

user-2 -->EC2-Support (EC2 ReadOnly)

user-3 --> EC2-Admin (EC2 Start/Stop)

### 4. Testasin eri käyttäjien oikeudet kirjautumalla sisään IAM-sign-in -URL:in avulla

Varmistin, että eri käyttöoikeudet vastaavat ryhmäpolitiikkoja:

user-1 näkee S3-bucketit, mutta ei EC2

user-2 näkee EC2-instanssit, mutta ei pysty muokkaamaan niitä

user-3:n oikeuksia ei erikseen testattu, mahdollisesti koska  EC2-Admin-ryhmään kuuluvana hänellä on laajimmat EC2-hallintaoikeudet (mm. instanssien käynnistys ja pysäytys)


### Tulokset ja oppi:

- IAM mahdollistaa yksityiskohtaisen käyttöoikeuksien hallinnan eri AWS-resursseihin

- Eri ryhmiin liitetyt politiikat helpottavat käyttäjähallintaa ja varmistaa yhtenäisen oikeustason

- Eri käyttäjäroolien (support/admin) testaaminen havainnollisti politiikkojen vaikutusta käytännössä 

- Avainkäsitteet: IAM Users, Groups, Policies, Managed vs Inline Policy, AWS Console Access





# 2) Configure Azure Role-Based Access Control


### 1. Ensiksi osoitin kyseiselle dev1 käyttäjälle roolin "Network Contributor" oikeassa Resource Groupissa.


<img width="1470" height="956" alt="Screenshot 2025-11-18 at 14 48 55" src="https://github.com/user-attachments/assets/5e2060dc-f47e-4d67-a883-2d3c1d68df30" />

Rooli hyväksytty, kuten näkyy.

<img width="1470" height="956" alt="Screenshot 2025-11-18 at 14 49 18" src="https://github.com/user-attachments/assets/48f062f9-7aed-492d-b433-916270acb2c3" />



### 2. Sen jälkeen kirjauduin Dev1 käyttäjälle uuteen selaimeen.

Sitten tein uuden Virtual Networkin Resource Groupiin, ja se on mahdollista Dev1 käyttäjällä, koska hänellä on network contributor oikeudet.

<img width="1470" height="956" alt="Screenshot 2025-11-18 at 14 55 15" src="https://github.com/user-attachments/assets/5fd8513f-3952-4a70-8a87-52572487b544" />

Sekin onnistuu nyt, kuten näkyy.

<img width="1470" height="956" alt="Screenshot 2025-11-18 at 14 55 39" src="https://github.com/user-attachments/assets/2319abc3-f30c-4fbe-ab0d-ba385af74c4f" />



### 3. Sen jälkeen oli tehtävänä kokeilla tehdä uutta Storage Accountia, mutta kuten virheilmoituksesta näkyy, käyttäjällä ei ole siihen oikeuksia.


<img width="1470" height="956" alt="Screenshot 2025-11-18 at 14 57 02" src="https://github.com/user-attachments/assets/063936d2-9461-4c21-ae82-469225735016" />


### 4. Seuraavana vuorossa oli tehdä "Custom Role", ja se tehtiin Admin käyttäjällä.

Avasin PowerShellin ja ajoin ohjeessa olevat komennot

Eli tämä komento listaa kaikki virtuaalikoneisiin liittyvät Azure-toiminnot ja näyttää ne siistissä taulukossa.

 
  $ Get-AzProviderOperation "Microsoft.Compute/virtualmachines/*"/ | FT Operation, Description -Autosize


Ja seuraava komento vie Virtual Machine Contributor roolin määritelmän JSON-tiedostoksi, jotta sitä voi tarkastella tai muokata myöhemmin (esim. uuden custom-roolin luomista varten).  


  $ Get-AzRoleDefinition -Name "Virtual Machine Contributor" | ConvertTo-Json | Out File $home\clouddrive\VMOperatorRole.json

Kun kyseiset komennot oli ajettu, ajoin ne uudelleen Classic Cloud Shellissa, koska se vaihtui sinne.


Sitten haettiin oikea hakemisto:

  $ cd $home\clouddrive

Sitten editoidaan koodia:

  $ code VMOperatorRole.json 

  Siinä vielä kuva valmiista paketista

  
<img width="1470" height="956" alt="Screenshot 2025-11-18 at 15 24 21" src="https://github.com/user-attachments/assets/f3d9ba9e-b9e7-41af-9995-67575726dbf2" />

  

### 3) Yhteenveto

Labrojen perusteella totesin, että suurin ero AWS IAM:n ja Azure RBAC:n välillä on se, että AWS on selvästi käyttäjä- ja politiikkakeskeinen, kun taas Azure on enemmän resurssi- ja roolikeskeinen. AWS:ssä oikeudet määritellään eri käyttäjille ja ryhmille tarkkojen JSON-politiikkojen kautta, mikä mahdollistaa tosi yksityiskohtaisen ja joustavan pääsynhallinnan. 

Azure puolestaan antaa oikeudet suoraan tiettyyn resurssialueeseen (kuten Resource Groupiin) selkeiden roolien kautta, mikä tekee käytöstä vähän yksinkertaisempaa ja läpinäkyvämpää. Testit osoittivat tämän: AWS-käyttäjät näki resurssit, mutta toimintoja rajoitettiin politiikoilla, kun taas Azure-käyttäjä pystyi toimimaan vain niissä resurssityypeissä, joita rooli suoraan tuki. Lisäksi Custom Rolen luonti oli Azurella vaiheistettu ja resurssipohjainen, kun taas AWS:n politiikat olivat suoremmin muokattavissa.
