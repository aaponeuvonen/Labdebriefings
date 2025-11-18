# 1) Introduction to AWS IAM

En ikävä kyllä jostain syystä ottanut screenshotteja tästä.

Labrassa tavoitteena ymmärtää käytännössä AWS IAM -palvelun perusteet: käyttäjien, ryhmien ja käyttöoikeuksien hallinta oikeassa AWS-ympäristössä

Vaiheet ohjeiden mukaan: 

## 1. Tutustuin ensiksi valmiiksi luotuihin IAM-käyttäjiin ja ryhmiin

Eri käyttäjiä oli: user-1, user-2, user-3

Ja eri ryhmiä: S3-Support, EC2-Support, EC2-Admin

## 2. Tarkastelin kyseisille ryhmille liitettyjä IAM-policeja (oikeuksia, menettelytapoja, politiikkoja)

S3-Support --> AmazonS3ReadOnlyAccess

EC2-Support --> AmazonEC2ReadOnlyAccess

EC2-Admin --> Oma Inline Policy (Start/Stop EC2 instances)


## 3. Sitten lisäsin käyttäjät oikeisiin ryhmiin liiketoimintatarpeiden mukaisesti kuten ohjeessa sanottiin 

user-1 --> S3-Support (S3 ReadOnly)

user-2 -->EC2-Support (EC2 ReadOnly)

user-3 --> EC2-Admin (EC2 Start/Stop)

## 4. Testasin eri käyttäjien oikeudet kirjautumalla sisään IAM-sign-in -URL:in avulla

Varmistin, että eri käyttöoikeudet vastaavat ryhmäpolitiikkoja:

user-1 näkee S3-bucketit, mutta ei EC2

user-2 näkee EC2-instanssit, mutta ei pysty muokkaamaan niitä

user-3:n oikeuksia ei erikseen testattu, mahdollisesti koska  EC2-Admin-ryhmään kuuluvana hänellä on laajimmat EC2-hallintaoikeudet (mm. instanssien käynnistys ja pysäytys)


## Tulokset ja oppi:

- IAM mahdollistaa yksityiskohtaisen käyttöoikeuksien hallinnan eri AWS-resursseihin

- Eri ryhmiin liitetyt politiikat helpottavat käyttäjähallintaa ja varmistaa yhtenäisen oikeustason

- Eri käyttäjäroolien (support/admin) testaaminen havainnollisti politiikkojen vaikutusta käytännössä 

- Avainkäsitteet: IAM Users, Groups, Policies, Managed vs Inline Policy, AWS Console Access





# 2) Configure Azure Role-Based Access Control


Assigned an Azure built-in role to a user.
Tested an Azure built-in role assignment.
Created a custom role.









# 3) Comparison
