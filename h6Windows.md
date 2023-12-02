## x) Lue ja tiivistä

## Edellinen windows-kotitehtävä

- Valitsin sannir-nimimerkillä githubissa toimivan kotitehtävä-raportin viime vuoden palvelinten hallinta toteutukselta.
- Asentaa Windowsiin saltin, jonka jälkeen luo tilan, jossa luo uuden tekstitiedoston.
- Käyttää grains.items funktiota, jolla saa tekniset tiedot nykyisestä koneestaan.
- Hän näyttää esimerkit portin kokeilusta Linuxilla ja Windowsilla. Molemmilla käyttöjärjestelmillä tulee näyttää ainakin yksi avoin ja yksi suljettu portti.
- Windowsilla porttitiedot komennolla:`netstat -ano` ja Linuxilla: `sudo ss -tulpn`
https://github.com/sannnir/h5-Windows

## Halonen, Rajala ja Ollikainen 2023: Installing Windows 10 on a virtual machine
- Valitaan uusi kone virtualboxissa ja valitaan versioksi "Windows 10 (64-bit)".
- Allokoidaan sopivan kokoinen kovalevy (Esimerkissä 50GB.)
- Asetetaan hyvä määrä muistia (esim. 8GB) ja lisätään 4 prosessoria.
- Käynnistetään kone ja asetetaan koneeseen ladattu asennustiedosto Virtualboxin pyynnöstä.
- Asennuksessa valitaan suomenkielinen näppäimistö, alue ja tehdään koneella oma domain.

## LSB Workgroup, The Linux Foundation 2015: Filesystem Hierarchy Standard
- FHS:n tarkoituksena on tukea esimerkiksi sovellusten ja skriptien välistä yhteentoimivuutta sekä lisätä näiden järjestelmien dokumentaation yhdenmukaisuutta.
- FSH mahdollistaa kyvyn ennustaa asennettujen tiedostojen ja hakemistojen sijainnit.
- Roothakemisto `/` on ensisijainen hierarkian juuri ja koko tiedostojärjestelmähierarkian juurihakemisto. Se sisältää seuraavat tärkeät alihakemistot:

* `/bin`: Välttämättömät käyttäjän komennot, jotka on oltava saatavilla yksittäisen käyttäjän tilassa. (esim. cat, ls, cp).
* `/boot`: Käynnistyslataustiedostot (esim. ytimet).
* `/dev`: Laite-tiedostot (esim. /dev/null, /dev/disk0, /dev/sda1).
* `/etc`: konfiguraatiotiedostot (esim. ohjelmistojen konfiguraatiotiedostot, joita muokkasimme viime kotitehtävässä).
* `/home`: Käyttäjien kotihakemistot, jotka sisältävät tallennetut tiedostot, henkilökohtaiset asetukset jne.
* `/lib`: Kirjastot, jotka ovat välttämättömiä /bin ja /sbin hakemistojen binääreille.

## a) Asenna Windows virtuaalikoneeseen.
- Käytin tämän tehtävän tekemiseen aikaisemmin mainittua ohjetta: "Halonen, Rajala ja Ollikainen 2023: Installing Windows 10 on a virtual machine". Latasin asennustiedoston Windowsin sivuilta: Win10, Enterprise evaluation, 64-bit. Tein uuden virtuaalikoneen VirtualBoxissa, Windows 10. Asetin muistiksi 8GB, lisäsin 2 prosessoria ja avasin koneen. Lisäsin asennustiedoston, kun virtualbox sitä pyysi. Valitsin suomenkielisen näppäimistön, alueen ja tein koneella oman domainin.

## Asenna Salt Windowsille. Osoita 'salt-call --local' komentoa ajamalla, että asennus on onnistunut.

- Ladataan Salt Windows-asennustiedosto Saltprojectin sivulta.
- Avataan komentokehoite as admin.
- Siirrytään lataukset kansioon
` cd C:\Users\Valtteri\Downloads`
- Käynnistetään salt-minion asennustiedosto.
`./Salt-Minion-3004.2-1-Py3-AMD64.msi`
- Asetin asennuksessa masterip:ksi koneen ip:n ja minioksi localhost.
- Kokeilen salttia komennolla: `salt-call --local cmd.run ”echo hello”`
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/0fc6dea9-7e3f-43bf-80c4-e9b1b58f7040)

## c) Kerää Windows-koneesta tietoa grains.items -toiminnolla. 

- Ensiksi keräämme kaikki tiedot koneesta komennolla `salt-call --local grains.items`.
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/91eac708-39da-436d-ab74-1423cfb30654)
- Voimme tämän jälkeen hakea grains-item - komennolla tiettyjä koneen tietoja. Haemme esimerkiksi prosessorin nimen sekä käyttöjärjestelmän. `cpu_model` ja `osfinger`. Koko komento: salt-call --local grains.item cpu_model osfinger.
- `cpu-model` palauttaa järjestelmän prosessori-mallin, jossa komento suoritetaan.
- `osfinger`  palauttaa käyttöjärjestelmän.
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/aaae0107-3e2d-4da7-863c-1b032661bf3b)

## d) Kokeile Saltin file -toimintoa Windowsilla.
- Teemme yksinkertaisen file-tilan, jossa luomme "valtteri"-käyttäjäkansioon uuden teksti tiedoston "testi.txt".
- salt-call --local state.single file.managed C:\Users\valtteri\testi.txt
- Tarkistetaan vielä, että komento teki oikeasti tiedoston.
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/07a728e3-7dbf-416e-a343-0f99ba38d233)
- Tarkistus dir-komennolla.
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/22ef56d3-a32b-4585-a260-e4828ba5944f)





