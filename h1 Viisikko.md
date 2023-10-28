# Viisikko

## Lue ja tiivistä Karvinen 2023: Create a Web Page Using Github, Karvinen 2023: Run Salt Command Locally

### Karvinen 2023: Create a Web Page Using Github

#### Repon Luominen
- Rekisteröidy Githubiin (https://github.com/)
- Klikkaa +-nappia oikeassa yläkulmassa ja valitse "Uusi repositorio/New repository".'

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/7c09919a-1bff-486a-802a-f3e753e717d5)

- Anna kuvaava nimi repositoriolle.
- Valitse vaihtoehto "Initialize this repository with a README".
- Valitse lisenssi (esim. GNU General Public License version 3).

Tässä esimerkki uudesta reposta, h0 tehtävä
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/515f0642-0721-4d3f-9cca-d42140fcfcc4)

#### Lisätään markdown-tiedosto

- Valitse "New file" repositorion päävalikosta.
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/05ffe01e-1331-4424-856a-d56822b859d3)

- Anna tiedostolle nimi jotain.md(tässä esimerkissä readme.md). Markdownin .md pääte tarkoittaa, että se muunnetaan automaattisesti web sivuksi.
- Hashtägien määrällä voimme määritellä Otsikon koon, kuten html-koodissa h1=# h2=##,jne. Koodi sisennetään neljällä välilyönnillä tai täbillä.
- URL-osoitteet muotoillaan automaattisesti, kuten http://TeroKarvinen.com

## Karvinen 2023: Run Salt Command Locally
Salt-komentoja voi ajaa paikallisesti ja nähdä tuloksen välittömästi. Tämä on hyödyllistä harjoittelua, testausta ja nopeaa asennusta varten.Salt-toiminnot toimii sekä Linuxissa että Windowsissa.
Käsittelemme artikkelin aiheita lopuissa tehtävissä.

## a) Asenna Salt (salt-minion) koneellesi.

Meidän tulee käyttää seuraavat komennot salt-minionin asennukseen.

$ sudo apt-get update
$ sudo apt-get -y install salt-minion

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/d307dc70-3a86-4f13-a0d4-aba230f7e77c)

Voimme tarkistaa, että meillä on salt-minion komennolla:
$ sudo salt-call --version

## b) Viisi tärkeintä. Näytä esimerkit viidestä tärkeimmästä Saltin tilafunktiosta: pkg, file, service, user, cmd. Analysoi ja selitä tulokset.

### pkg.installed - sovelluksen asentaminen

- Sovelluksen asentaminen
  $ sudo salt-call --local -l info state.single pkg.installed tree

- Sovelluksen poistaminen
  $ sudo salt-call --local -l info state.single pkg.removed tree

  ![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/01fe0852-9ef6-4718-9365-37fb4cfec9d3)

### file.managed - Tiedoston tarkistaminen

- Tiedoston luominen
  $ sudo salt-call --local -l info state.single file.managed /tmp/hellovaltteri
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/4ca44f8d-bcfe-4c8b-bbe7-1d6d18e2a401)

- Tiedoston sisällön muokkaaminen
$ sudo salt-call --local -l info state.single file.managed /tmp/hellovaltteri contents="foo"
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/0b8adaa7-714c-469f-9dc5-24a1e5c32117)

- Tiedoston poistaminen
$ sudo salt-call --local -l info state.single file.absent /tmp/hellotero
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/71c2e823-2b46-4d8e-b539-a7bcb29c0a8a)


### service.running - apache2-demonin käynnistys

$ sudo apt install apache2
$ sudo salt-call --local -l info state.single service.running apache2 enable=True
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/b158db2b-e12a-4065-9a31-214d6d9c01f8)

- demonin sulkeminen
$ sudo salt-call --local -l info state.single service.dead apache2 enable=False
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/87ee7ae3-eeea-403c-ae3e-9b5966abcf95)









