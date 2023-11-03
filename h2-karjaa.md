## Slater 2017: What is the definition of "cattle not pets"?.

- "Lemmikit" ja "Karja" ovat käsitteitä, jotka kuvastavat erilaista tapaa suhtautua palvelimiin ja niiden hallintaan.

- Lemmikit ovat palvelimia tai palvelinpareja, joita pidetään korvaamattomina ja ainutlaatuisina järjestelminä, joita ei koskaan saa olla poissa käytöstä. Niitä hallinnoidaan ja rakennetaan yleensä manuaalisesti.

- Karja taas koostuu yli kahden palvelimen ryhmistä, jotka on rakennettu automatisoiduilla työkaluilla. Nämä ryhmät on suunniteltu kestämään vikoja, ja yksittäiset palvelimet eivät ole korvaamattomia.

- Bias ja Baker kokee, että on tärkeää siirtyä pois perinteisestä tapaa käsitellä palvelimia "Ainutlaatuisina lumipalloina" kohti mallia, jossa palvelimet ovat numeroituja ja korvattavissa tarvittaessa.

- Lopuksi, palvelimen poistaminen käytöstä ei välttämättä ole paras vaihtoehto. Sen sijaan on hyödyllistä "jäädyttää" palvelin, esimerkiksi docker pause -toiminnolla, jotta voidaan suorittaa juurisyyn analysointi tapahtuma- tai ongelmanhallintaprosessin osana.

- Lähde: https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets


## Karvinen 2017: Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds (Suosittelen koneeksi 'vagrant init debian/bullseye64')

31 sekunnin virtuaalikoneen asennus

Asenna VirtualBox ja Vagrant
$ sudo apt-get update
$ sudo apt-get -y install vagrant virtualbox

Uudella koneella
$ vagrant init debian/bullseye64
$ vagrant up
$ vagrant ssh

- Lähde: https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/ 

## Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves
### a) Asenna Vagrant. (Windows)
- Ensiksi ladataan Vagrantin asennustiedosto Hashicorpin sivuilta, valitaan Windows ja AMD64-arkkitehtuuri. Linkki: https://developer.hashicorp.com/vagrant/downloads

- Käynnistetään asennusohjelma ja asennetaan se. Asennuksen tarkistus komennolla komentokehoitteessa:
b) Yksi maankiertäjä. Asenna yksi kone Vagrantilla, ota siihen SSH-yhteys, osoita että netti toimii.
c) Oma orjansa. Asenna Salt herra ja orja samalle koneelle.
d) Asenna Saltin herra-orja arkkitehtuuri toimimaan verkon yli. (Verkko voi olla virtuaalinen verkko paikallisten virtuaalikoneiden välillä, kuten muissakin alakohdissa)
e) Aja useita idempotentteja (state.single) komentoja verkon yli.
f) Kerää teknistä tietoa orjista verkon yli (grains.item)
g) Aja shell-komento orjalla verkon yli.
h) Hello, IaC. Tee infraa koodina kirjoittamalla /srv/salt/hello/init.sls. Aja tila jollekin orjalle. Tila voi esimerkiksi tehdä esimerkkitiedoston johonkin hakemistoon. Testaa toisella komennolla, että pyytämäsi muutos on todella tehty.
