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
### a) Asenna Vagrant. (Linux)
- Ensiksi ladataan Vagrantin asennustiedosto

Linkki: https://developer.hashicorp.com/vagrant/downloads
$ sudo apt update
$ sudo apt install vagrant


- Asennuksen tarkistus komennolla komentokehoitteessa: vagrant version
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/024c7f46-fe01-4711-ac2b-8ed443f4eb3a)


## Asenna yksi kone Vagrantilla, ota siihen SSH-yhteys, osoita että netti toimii.

$ sudo apt-get update
$ sudo apt-get -y install virtualbox vagrant micro

$ mkdir saltdemo; cd saltdemo
$ nano Vagrantfile

- Käytämme valmista vagrantfileä virtuaalikoneiden tekemiseen:
```
" # -*- mode: ruby -*-
# vi: set ft=ruby :
# Copyright 2014-2023 Tero Karvinen http://TeroKarvinen.com

$minion = <<MINION
sudo apt-get update
sudo apt-get -qy install salt-minion
echo "master: 192.168.12.3">/etc/salt/minion
sudo service salt-minion restart
echo "See also: https://terokarvinen.com/2023/salt-vagrant/"
MINION

$master = <<MASTER
sudo apt-get update
sudo apt-get -qy install salt-master
echo "See also: https://terokarvinen.com/2023/salt-vagrant/"
MASTER

Vagrant.configure("2") do |config|
	config.vm.box = "debian/bullseye64"

	config.vm.define "t001" do |t001|
		t001.vm.provision :shell, inline: $minion
		t001.vm.network "private_network", ip: "192.168.12.100"
		t001.vm.hostname = "t001"
	end

	config.vm.define "t002" do |t002|
		t002.vm.provision :shell, inline: $minion
		t002.vm.network "private_network", ip: "192.168.12.102"
		t002.vm.hostname = "t002"
	end

	config.vm.define "tmaster", primary: true do |tmaster|
		tmaster.vm.provision :shell, inline: $master
		tmaster.vm.network "private_network", ip: "192.168.12.3"
		tmaster.vm.hostname = "tmaster"
	end
end ```

$ vagrant up

- Sain jostain syystä virheen: Error while connecting to Libvirt: Error making a connection to libvirt URI qemu
- Ratkaisu: $ sudo vagrant up. Ongelman ratkaisemikseksi asensin libvirt:in, mutta en usko sen toimineen, koska sama virhe tuli uudestaan. Kokeilin testiksi sudo:a, jolla ongelma ratkaistiin.

$ sudo ssh vagrant ssh tmaster

- Tämän komennon jälkeen saimme yhteyden master-koneeseen!
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/a978bed6-8f45-4514-bfaf-c696e49ae994)

- Kokeillaan internettiä, voimme todeta sen toimivan!:
$ ping google.com
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/b4d6232b-9b81-43da-9cae-547bd121206e)



c), d) Oma orjansa. Asenna Salt herra ja orja samalle koneelle.

-Asensimme salt:in ylempänä, jossa teimme myös verkon koneiden välille.
-Voimme testata koneiden välistä yhteyttä ja käyttää komentoja

e) Aja useita idempotentteja (state.single) komentoja verkon yli.
- Kokeillaan komentoa $ $ sudo salt '*' state.single file.managed '/tmp/see-you-at-github-windocode'
- Tämä luo virtuaalikoneille tiedoston "see-you-at-github-windocode'
- Tehdään se uudestaan: kommenteissa ilmoitetaan tiedoston olevan jo olemassa. Succeeded: 1 = Idempotentti
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/02da2fe6-2681-4044-8c4e-0ea3c7a4df3f)

- Poistetaan kyseinen tiedosto virtuaalikoneilta $ $ sudo salt '*' state.single file.removed '/tmp/see-you-at-github-windocode'
-Toistetaan komento: Logissa tieto: Tiedostoa ei olemassa. Voimme todeta komennon olevan idempotentti.
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/0147ee87-a8dd-4403-94a8-4e807a2d2417)


f) Kerää teknistä tietoa orjista verkon yli (grains.item)
g) Aja shell-komento orjalla verkon yli.
h) Hello, IaC. Tee infraa koodina kirjoittamalla /srv/salt/hello/init.sls. Aja tila jollekin orjalle. Tila voi esimerkiksi tehdä esimerkkitiedoston johonkin hakemistoon. Testaa toisella komennolla, että pyytämäsi muutos on todella tehty.
