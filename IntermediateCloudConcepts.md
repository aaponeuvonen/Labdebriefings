# Lab 6 - Scale & Load Balance your Architecture (AWS)

#### Labran tavoitteena oli rakentaa skaalautuva ja vikasietoinen AWS-infra

#### Labrassa käytettiin:

- Elastic Load Balancing (ELB)

- Auto Scaling

- Amazon Machine Images (AMI)

- CloudWatch valvontaa, mikä itsellä ei toiminut hälytysten muodossa


#### Ensiiksi luotiin AMI olemassa olevasta EC2 instanssista 

- Luotiin kopio / snapshot

-->

- Luotua AMI pohjaa käytettiin mallina kaikille uusille Auto Scalingin luomille instansseille


<img width="1169" height="78" alt="Screenshot 2025-11-28 at 17 54 44" src="https://github.com/user-attachments/assets/f8319419-62ed-4552-a972-1285f14cd9f4" />


#### Loin Elastic Load Balancerin 

- Määritin sen jakamaan HTTP-liikenteen tasaisesti useille EC2 instansseille

- Tein Health Check asetukset kuten: HTTP:80/

- Sekä lisäsin Target Groupin, johon Auto Scaling rekisteröi sitten ne instansit

#### Elastic Load Balancing lisää viansietokykyä, jakamalla kuorman tasaisesti 


<img width="1175" height="92" alt="Screenshot 2025-11-28 at 18 07 17" src="https://github.com/user-attachments/assets/fbd63145-519a-4f40-a41d-b08a9ffe9f6d" />


#### Sitten tein Launch Templaten joka sisälsi:

- Käytettävä AMI

- Instanssityyppi tässä tapauksessa t2.micro

- Securtiy Group

- VPC / subnet asetukset

#### Kyseinen Template kertoo Auto Scalingille tarkemmin millaisia instansseja halutaan sen luovan


<img width="1283" height="71" alt="Screenshot 2025-11-28 at 18 15 22" src="https://github.com/user-attachments/assets/350f2ba8-8333-4795-90ff-9f5a37664459" />


#### Sitten Auto Scaling Group (ASG) määritykset:

- Minimissä 1 instanssi

- Toivottu määrä 1 instanssi

- Maksimissa 3 instanssia

- Liitin sen äskettäin luotuun Load Balancer target groupiin

- Sitten Availability Zonet jossa instanssit saa rullata


  <img width="1376" height="50" alt="Screenshot 2025-11-28 at 18 26 59" src="https://github.com/user-attachments/assets/baf3cff5-92d4-4c15-b2c1-0936bbcc221c" />


#### Auto Scaling päälle 

- CPU utilization sääntö

- Jos CPU utilization yli 70% → lisää instanssi
  
- Jos CPU utilization alle 30% → poista instanssi


#### CloudWatch hälytykset 

Näitä en itse onnistunut tekemään, vaikka toki kriittinen osa Auto Scalingia, sillä hälytykset aktivoi sen


## Yhteenvetona

#### - Tajusin tässä harjoituksessa eniten ehkä kaikista, sillä kaikki osat olivat isossa roolissa tässä harjoituksessa, ja siten oli helppo hahmottaa kokonaisuus

#### - ASG ja ELB yhdessä mahdollistaa automaattisen skaalaamisen ja vikasietoisuuden

#### - AMI:en avulla uusista instansseista saadaan identtisiä vanhojen kanssa

#### - Launch Template ja ASG määrittelevät tarkasti instanssien tyypin, määrän ja sijoittelun 

#### - CloudWatch hälytykset ovat hyvin keskeisiä Auto Scalingin toiminnassa, vaikka ne eivät tässä labrassa onnistuneet 

#### - Käytännössä opin, miten AWS-infra voidaan rakentaa skaalautuvaksi ja resilientiksi



# Azure labraa ei enää ole :(


