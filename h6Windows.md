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

- `/bin`: Välttämättömät käyttäjän komennot, jotka on oltava saatavilla yksittäisen käyttäjän tilassa. (esim. cat, ls, cp).
- `/boot`: Käynnistyslataustiedostot (esim. ytimet).
- `/dev`: Laite-tiedostot (esim. /dev/null, /dev/disk0, /dev/sda1).
- `/etc`: konfiguraatiotiedostot (esim. ohjelmistojen konfiguraatiotiedostot, joita muokkasimme viime kotitehtävässä).
- `/home`: Käyttäjien kotihakemistot, jotka sisältävät tallennetut tiedostot, henkilökohtaiset asetukset jne.
- `/lib`: Kirjastot, jotka ovat välttämättömiä /bin ja /sbin hakemistojen binääreille.


